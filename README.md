# favicons-icons
## Introdução
Hoje precisamos gerar muitas imagens e inserir muitas metatags, cada uma para um dispositivo diferente e para cada sistema diferente, seja iOS, Android, Windows.

E não são somente muitas imagens, também temos os arquivos de configuração, a Microsoft com o ``` ieconfig.xml ```, cujo o propósito é o de criar "live tiles" dinamicamente no IE10/11 Metro/Tile. [[1]](https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx) e o Chrome para Android com o ``` manifest.json ``` só que não em xml mas sim em JSON. [[2]](https://developers.google.com/web/updates/2014/11/Support-for-installable-web-apps-with-webapp-manifest-in-chrome-38-for-Android) [[3]](https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/)

A tarefa não foi apenas gerar/criar imagens mas sim aprender muita coisa nova e automatizar esse processo para a plataforma existente.

## Desenvolvimento
Precisamos definir quais dispositivos queremos atingir, cada negócio pode priorizar um ou outro (como somente para iPhone Retina iOS6+ ou qualquer Smarthphones mas não Tablets), depois geramos as imagens do ícones. Segui com a premissa de que a imagem do ícone seria enviada via POST (por ajax) através de um formulário no site.

### Tamanhos
Utilizei uma lista de tuplas para definir, facilitando qualquer mudança em largura e altura futuramente.

Favicons. [[16]](https://www.w3.org/2005/10/howto-favicon) [[17]](https://en.wikipedia.org/wiki/Favicon)
Um arquivo com auto-resize ` -define icon:auto-resize=16,32,48,56,64 `. [[12]](http://www.imagemagick.org/script/command-line-options.php#define)
```
sizes = [16,32,48,56,64]
temp_file = NamedTemporaryFile(delete=False)

# -define icon:auto-resize=
os.system('convert %s -background transparent -clone 0 -define icon:auto-resize=%s -delete 0 %s' % (upload_file,sizes,temp_file.name))
```
```
ICO_SIZES = [
    (16, 16),
    (32, 32),
    (48, 48),
    (56, 56),
    (64, 64)
]
```
Windows ícones: pequeno, médio, wide, grande. [[1]](https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx)
```
WINDOWS_PNG_SIZES = [
    (128, 128),
    (270, 270),
    (558, 270),
    (558, 558)
 ]
```
Chrome e Opera ícones devem ser base 48px. [[4]](https://developers.google.com/web/fundamentals/design-and-ui/browser-customization/)
```
 PNG_SIZES = [48, 96, 144, 192]
 ```
iOS Human Interface. [[8]](https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/)

| Device or context | Icon size |
| ---|--- |
| iPhone 6s Plus, iPhone 6 Plus | 180px by 180px |
| iPhone 6s, iPhone 6, iPhone SE | 120px by 120px |
| iPad Pro | 167px by 167px |
| iPad, iPad mini | 152px by 152px |
| App Store | 1024px by 1024px |

Browser customization. [[4]](https://developers.google.com/web/fundamentals/design-and-ui/browser-customization/)
Manifest, depth ser ignorado se inserido no arquivo. [[7]](https://developer.mozilla.org/en-US/docs/Web/Manifest)

Nossa configuraço final dos ícones ficou com os seguintes tamanhos em png:
```
PNG_SIZES = [
    (32, 32),
    (48, 48), # Chrome e Opera (Manifest)
    (56, 56),
    (58, 58),
    (72, 72), # Manifest
    (76, 76), # Safari
    (87, 87),
    (96, 96), # Chrome e Opera (Manifest)
    (120, 120), # Safari e iPhone 6s, iPhone 6, iPhone SE
    (128, 128),
    (144, 144), # Chrome e Opera (Manifest)
    (152, 152), # Safari e iPad, iPad mini
    (168, 168), # Manifest
    (167, 167), # iPad Pro
    (180, 180), # iPhone 6s Plus, iPhone 6 Plus
    (192, 192), # Chrome e Opera (Manifest) - icon in the highest resolution we need it for
    (195, 195),
    (196, 196),
    (228, 228)
]
```
Mathias Bynens. [[9]](https://mathiasbynens.be/notes/touch-icons)
```
COMPRESED_PNG_SIZES = [
    (72, 72),
    (76, 76),
    (114, 114),
    (120, 120),
    (128, 128),
    (144, 144),
    (152, 152),
    (180, 180),
    (192, 192)
] 
```

### convert - ImageMagick
Podemos gerar a imagem do ícone de qualquer arquivo png, jpg, jpeg, ico ou svg, precisamos indicar a cor do background, indicar transparência, indicar o tamanho que queremos e o arquivo de destino. [[11]](http://www.imagemagick.org/Usage/thumbnails/#favicon) [[12]](http://www.imagemagick.org/script/command-line-processing.php) 

Para forçarmos a inclusão de um fundo em uma imagem de input com fundo transparente.  [[18]](http://www.imagemagick.org/script/command-line-options.php#flatten)
```
size = '%sx%s' % (s,s)
temp_file = NamedTemporaryFile(delete=False)

# Transparência
os.system('convert %s -background transparent -clone 0 -resize %s -delete 0 %s' % (upload_file,size,temp_file.name))

# Background com cor branca
os.system('convert %s -background white -flatten -clone 0 -resize %s -delete 0 %s' % (upload_file,size,temp_file.name))
```

### Metatags HTML
Até o momento (01/2017) nenhum browser suporta as tags `theme-color` e `background-color`. Apenas no mobile o Android Webview, Opera mobile e Chrome para Android. Para o IE, Safari e Firefox não encontrei documentação.
```
<!-- icon in the highest resolution -->
<link rel="icon" sizes="192x192" href="favicon.png">

<!-- Safari -->
<!-- In its simplest form: -->
<link rel="apple-touch-icon" href="apple-touch-icon.png">
<!-- For iPhone 6 Plus with @3× display: -->
<link rel="apple-touch-icon-precomposed" sizes="180x180" href="apple-touch-icon-180x180-precomposed.png">
<!-- For iPad with @2× display running iOS ≥ 7: -->
<link rel="apple-touch-icon-precomposed" sizes="152x152" href="apple-touch-icon-152x152-precomposed.png">
<!-- For iPad with @2× display running iOS ≤ 6: -->
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="apple-touch-icon-144x144-precomposed.png">
<!-- For iPhone with @2× display running iOS ≥ 7: -->
<link rel="apple-touch-icon-precomposed" sizes="120x120" href="apple-touch-icon-120x120-precomposed.png">
<!-- For iPhone with @2× display running iOS ≤ 6: -->
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="apple-touch-icon-114x114-precomposed.png">
<!-- For the iPad mini and the first- and second-generation iPad (@1× display) on iOS ≥ 7: -->
<link rel="apple-touch-icon-precomposed" sizes="76x76" href="apple-touch-icon-76x76-precomposed.png">
<!-- For the iPad mini and the first- and second-generation iPad (@1× display) on iOS ≤ 6: -->
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="apple-touch-icon-72x72-precomposed.png">
<!-- For non-Retina iPhone, iPod Touch, and Android 2.1+ devices: -->
<link rel="apple-touch-icon-precomposed" href="apple-touch-icon-precomposed.png"><!-- 57×57px -->

<!-- multiple icons for IE -->
<meta name="msapplication-square310x310logo" content="icon_largetile.png">

<!-- For Chrome for Android: -->
<link rel="icon" sizes="192x192" href="touch-icon-192x192.png">

<!-- iOS -->
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<link rel="apple-touch-startup-image" href="icon.png">

<!-- Chrome, Firefox OS and Opera -->
<meta name="theme-color" content="#4285f4">
```

## Referências
1. [https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx](https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx)
2. [https://developers.google.com/web/updates/2014/11/Support-for-installable-web-apps-with-webapp-manifest-in-chrome-38-for-Android](https://developers.google.com/web/updates/2014/11/Support-for-installable-web-apps-with-webapp-manifest-in-chrome-38-for-Android)
3. [https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/](https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/)
4. [https://developers.google.com/web/fundamentals/design-and-ui/browser-customization/](https://developers.google.com/web/fundamentals/design-and-ui/browser-customization/)
5. [https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)
6. [https://gist.github.com/tfausak/2222823](https://gist.github.com/tfausak/2222823)
7. [https://developer.mozilla.org/en-US/docs/Web/Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest)
8. [http://www.w3.org/TR/appmanifest/](http://www.w3.org/TR/appmanifest/)
- [https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/](https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/)
9. [https://mathiasbynens.be/notes/touch-icons](https://mathiasbynens.be/notes/touch-icons)
10. [https://html.spec.whatwg.org/multipage/semantics.html#rel-icon](https://html.spec.whatwg.org/multipage/semantics.html#rel-icon)
11. [http://www.imagemagick.org/Usage/thumbnails/#favicon](http://www.imagemagick.org/Usage/thumbnails/#favicon)
12. [http://www.imagemagick.org/script/command-line-processing.php](http://www.imagemagick.org/script/command-line-processing.php)
13. [http://docs.wand-py.org/en/0.4.1/wand/image.html#wand.image.Image](http://docs.wand-py.org/en/0.4.1/wand/image.html#wand.image.Image)
14. [http://docs.wand-py.org/en/0.4.1/guide/read.html#hint-file-format](http://docs.wand-py.org/en/0.4.1/guide/read.html#hint-file-format)
15. [https://w3c.github.io/manifest/](https://w3c.github.io/manifest/)
16. [https://www.w3.org/2005/10/howto-favicon](https://www.w3.org/2005/10/howto-favicon)
17. [https://en.wikipedia.org/wiki/Favicon](https://en.wikipedia.org/wiki/Favicon)
18. [http://www.imagemagick.org/script/command-line-options.php#flatten](http://www.imagemagick.org/script/command-line-options.php#flatten)
