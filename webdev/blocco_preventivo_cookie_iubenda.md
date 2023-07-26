# BLOCCO PREVENTIVO COOKIE IUBENDA

## WORDPRESS

1. Installare il plugin [Iubenda Cookie Solution](https://wordpress.org/plugins/iubenda-cookie-solution/) e attivarlo

2. Modificare gli script javascript del sito in modo che vengano caricati solo dopo che il banner è stato accettato (segui esempi qui sotto)

### Script che caricano codice esterno

```html
<!-- Prima -->
<script async src="https://www.googletagmanager.com/gtag/js?id=XXX"></script>

<!-- Dopo -->
<script async class="_iub_cs_activate" type="text/plain" data-iub-purposes="3" data-suppressedsrc="https://www.googletagmanager.com/gtag/js?id=XXX"></script>
```

### Script che caricano codice inline

```html
<!-- Prima -->
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
</script>

<!-- Dopo -->
<script class="_iub_cs_activate-inline" type="text/plain" data-iub-purposes="3">
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
</script>
```
