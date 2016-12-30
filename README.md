# favicons
Os favicons não são mais como antes, favicon.ico no tamanho 16x16, agora existem diferentes usos, muitos tamanhos e diferentes designs. Um exemplo é o Apple Touch Icon para iOS que é usado quando o visitante acrescenta o site na tela de ínicio. O Android e Windows 8 seguem a mesma idéia mas cada um a sua maneira.

Agora se queremos as coisas bem feitas, precisamos gerar muitas imagens para os ícones, no nosso caso foram 22 favicons.

E não são somente muitas imagens, no Windows 8 e IE11 a Microsoft introduziu o ``` browserconfig.xml ``` com o propósito de criar "live tiles" dinamicamente. [Ref. 1](https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx).

O Android Chrome M39 também veio com um ``` manifest ``` só que não xml mas sim JSON.

Resumindo, não foi apenas gerar/criar imagens mas sim aprender muita coisa nova.

Se a preocupação fosse somente com o tamanho das imagens e sua geração ok, mas temos muitas outras coisas para nos preocupar. Cada plataforma tem suas próprias limitações de designer.

# Referências
- [Apple 1](https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/)
- [Apple 2](https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)
- [Microsoft](https://msdn.microsoft.com/library/dn455106(v=vs.85).aspx)
- [Mathias Byens](https://mathiasbynens.be/notes/touch-icons)
- [What WG](https://html.spec.whatwg.org/multipage/semantics.html#rel-icon)
