---
layout: page
title:  "03 - Rails snippets"
---

## ACTIVESTORAGE S3 WITH CUSTOM STORAGE CLASS

```yml
local:
  service: Disk
  root: <%= Rails.root.join("storage") %>

test:
  service: Disk
  root: <%= Rails.root.join("tmp/storage") %>

amazon_s3:
  service: S3
  bucket: bucket_name
  upload:
    storage_class: INTELLIGENT_TIERING # STANDARD REDUCED_REDUNDANCY INTELLIGENT_TIERING STANDARD_IA ONEZONE_IA GLACIER DEEP_ARCHIVE
```

## GENERATE SESSION COOKIE FROM SECRET KEY BASE

```ruby
require 'active_support'
require 'active_support/message_encryptor'
require 'cgi'

def generate_session_cookie(data_hash)
  secret_key_base = Rails.application.credentials.secret_key_base
  
  # Create the session hash with required metadata
  session_data = {
    "session_id" => SecureRandom.hex(16),
    "_csrf_token" => SecureRandom.base64(32)
  }.merge(data_hash)

  # Configure the key generator
  key_generator = ActiveSupport::KeyGenerator.new(secret_key_base, iterations: 1000)
  secret = key_generator.generate_key('authenticated encrypted cookie')
  sign_secret = key_generator.generate_key('signed encrypted cookie')

  # Create the encryptor
  encryptor = ActiveSupport::MessageEncryptor.new(
    secret[0, 32],
    sign_secret[0, 32],
    cipher: 'aes-256-gcm',
    serializer: JSON
  )

  # Generate session key
  session_key = Rails.application.config.session_options[:key] || '_session_id'

  # Encrypt the session data
  encrypted_data = encryptor.encrypt_and_sign(session_data)
  
  # URL encode the cookie value
  encoded_cookie = CGI.escape(encrypted_data)
  
  {
    cookie_string: "#{session_key}=#{encoded_cookie}",
    curl_command: "curl https://domain.com --cookie \"#{session_key}=#{encoded_cookie}\""
  }
end

# Usage example
result = generate_session_cookie(user_id: 1)
puts result[:curl_command]
```