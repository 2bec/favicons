# favicons
## Introdução
Os favicons não são mais como antes (uns 10 anos atrás), onde bastava um arquivo .ico de 16x16. 
Agora existem diferentes usos, variados tamanhos e diferentes designs. Um exemplo é o Apple Touch Icon para iOS que é usado quando o visitante acrescenta o site na tela de ínicio. O Android e Windows seguem a mesma idéia mas cada um a sua maneira.

Hoje precisamos gerar muitas imagens e inserir muitas metatags, cada uma para um dispositivo diferente e para cada sistema diferente, seja iOS, Android, Windows.

E não são somente muitas imagens, também temos os arquivos de configuração, a Microsoft com o ``` ieconfig.xml ```, cujo o propósito é o de criar "live tiles" dinamicamente no IE10/11 Metro/Tile. [[1]](https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx)

O Chrome para Android também veio com um ``` manifest ``` só que não em xml mas sim em JSON. [[2]](https://developers.google.com/web/updates/2014/11/Support-for-installable-web-apps-with-webapp-manifest-in-chrome-38-for-Android) [[3]](https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/)

Resumindo, não foi apenas gerar/criar imagens mas sim aprender muita coisa nova.

## Desenvolvimento
Definidos os tamanhos que precisamos (cada negócio pode priorizar um ou outro formato e tamanho) podemos gerar as imagens do ícones. Segui com a premissa de que a imagem do ícone seria enviada via POST através de um formulário no site.

### Tamanhos
Windows ícones: pequeno, médio, wide, grande. [[1]](https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx)
Chrome e Opera ícones devem ser base 48px, por exemplo: 48px, 96px, 144px. [[4]](https://developers.google.com/web/fundamentals/design-and-ui/browser-customization/)
iOS Human Interface [[8]](https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/)
```
ICO_SIZES = [16, 32, 48, 64]
PNG_SIZES = [32, 48, 56, 58, 76, 87, 96, 120, 128, 144, 152, 167, 180, 192, 195, 196, 228]
WINDOWS_PNG_SIZES = [128, 270, (558, 270), 558]
FILEED_PNG_SIZES = [128, ] 
```

### convert - ImageMagick
Gerando a imagem do ícone de qualquer arquivo png ou jpg (jpeg), precisamos indicar a transparência do background, o tamanho que queremos e o arquivo de destino, onde a imagem do ícone será armazenada.
```
def generate_icons(request, sizes=[16,32,48,64]):
   if request.method == 'POST':
      upload_file = request.FILES['imagem']

      for s in sizes:
         size = '%sx%s' % (s,s)
         temp_file = NamedTemporaryFile(delete=False)
         os.system('convert %s -background transparent -clone 0 -resize %s -delete 0 %s' % (upload_file,size,temp_file.name))
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
