---
layout: default
---

### Conte&uacute;dos

- [Declare um doctype](#doctype)
- [Matem&aacute;tica do Box Model](#box-model-math)
- [Unidades `rem` e Mobile Safari](#rems-mobile-safari)
- [Floats primeiro](#floats-first)
- [Floats e clearing](#floats-clearing)
- [Floats e altura computada](#floats-computed-height)
- ["float" &eacute; nível de bloco](#floats-block-level)
- [Colapso de margens verticais adjacentes](#vertical-margins-collapse)
- [Estilizando linhas de tabela](#styling-table-rows)
- [Firefox e bot&otilde;es `<input>`](#buttons-firefox)
- [Sempre especifique um `type` para `<button>`s](#buttons-type)
- [Limite de seletor do Internet Explorer](#ie-selector-limit)
- ["position" explicada](#position-explained)
- ["position" e "width"](#position-width)
- [Posição "fixed" e transforms](#position-transforms)


<a name="doctype"></a>
### Declare um doctype
Sempre inclua um doctype. É recomendado o doctype simples de HTML5:

```html
<!DOCTYPE html>
```

Esquecer o doctype pode causar problemas com tabelas malformatadas, inputs e mais.


<a name="box-model-math"></a>
### Matem&aacute;tica do Box Model
Elementos que t&ecirc;m `width` especificada se tornam *maiores* quando eles t&ecirc;m `padding` e/ou `border-width`. Para evitar esses problemas, fa&ccedil;a o uso do agora comum [`box-sizing: border-box;` reset](http://www.paulirish.com/2012/box-sizing-border-box-ftw/).

<a name="rems-mobile-safari"></a>
### Unidades `rem` e Mobile Safari
Apesar de Mobile Safari suportar o uso de `rem`s em todos os valores de propriedades, ele n&atilde;o lida muito bem quando `rem`s s&atilde;o usados com media queries e fica piscando infinitamente o texto da p&aacute;gina em tamanhos diferentes.

Por enquanto, use `em`s no lugar de `rem`s.

```css
html {
  font-size: 16px;
}

/* Causa o "flashing bug" no Mobile Safari */
@media (min-width: 40rem) {
  html {
    font-size: 20px;
  }
}

/* Funciona lindamente no Mobile Safari */
@media (min-width: 40em) {
  html {
    font-size: 20px;
  }
}
```

**Ajuda!** Se voc&ecirc; tiver um link para um relat&oacute;rio de bug Webkit ou Apple sobre isso, envie para que seja inclu&iacute;do aqui. N&atilde;o temos certeza onde reportar isso j&aacute; que s&oacute; se aplica a Mobile, n&atilde;o Desktop, Safari.
 

<a name="floats-first"></a>
### Floats primeiro
Elementos com `float` devem sempre vir primeiro na ordem do documento. Elementos com `float` precisam de alguma coisa para envolv&ecirc;-los ("wrap around"), do contr&aacute;rio podem causar um efeito "step down", aparecendo abaixo do conte&uacute;do.

```html
<div class="parent">
  <div class="float">Float</div>
  <div class="content">
    <!-- ... -->
  </div>
</div>
```

<a name="floats-clearing"></a>
### Floats e clearing
Se voc&ecirc; usa `float` em algo, voc&ecirc; *provavelmente* precisa usar `clear` nisso. Qualquer conte&uacute;do que segue um elemento com `float` vai envolver ("wrap around") aquele elemento at&eacute; que ele fique "cleared". Para dar clear em floats, use alguma das t&eacute;cnicas a seguir.

Use a [micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/) para fazer o clear de floats com uma classe separada.

```css
.clearfix:before,
.clearfix:after {
  content: " ";
  display: table;
}
.clearfix:after {
  clear: both;
}
```
Alternativamente, especifique `overflow` com `auto` ou `hidden` no elemento-pai.

```css
.parent {
  overflow: auto; /* clearfix */
}
.other-parent {
  overflow: hidden; /* clearfix */
}
```
Tenha cuidado, pois `overflow` pode causar alguns efeitos colaterais, tipicamente em rela&ccedil;&atilde;o a elementos posicionados dentro do elemento-pai.

**Pro-Tip!** Mantenha seu futuro voc&ecirc; e seus colegas de trabalho felizes incluindo um coment&aacute;rio do tipo `/* clearfix */` quando der o clear em floats, j&aacute; que a propriedade pode ser usada por outras razões.

<a name="floats-computed-height"></a>
### Floats e altura computada
Um elemento-pai que tem somente conte&uacute;do float ir&aacute; ter uma altura computada `height: 0;`. Adicione um clearfix a esse elemento-pai para for&ccedil;ar os navegadores a computar a altura.

<a name="floats-block-level"></a>
### "float" &eacute; nível de bloco
Elementos com `float` automaticamente se tornam `display: block;`. **N&atilde;o especifique ambos.**

```css
.element {
  float: left;
  display: block; /* Não necessário */
}
```

**Fato engra&ccedil;ado:** Anos atr&aacute;s, n&oacute;s *tínhamos* que especificar `display: inline;` para a maioria dos floats funcionarem apropriadamente em IE6 e evitar o [double margin bug](http://www.positioniseverything.net/explorer/doubled-margin.html). Mas s&atilde;o &aacute;guas passadas.

<a name="vertical-margins-collapse"></a>
### Colapso de margens verticais adjacentes
Margens superiores e inferiores podem e ir&atilde;o colapsar em muitas situa&ccedil;&otilde;es, mas nunca por elementos posicionados com float ou absolutamente. [Leia esse artigo na MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing) para saber mais.

**Margens horizontais adjacentes nunca colapsam**.


<a name="styling-table-rows"></a>
### Estilizando linhas de tabela
Linhas de tabela, `<tr>`s, n&atilde;o recebem `border`s a menos que você especifique `border-collapse: collapse;` no elemento-pai `<table>`. Al&eacute;m disso, se o `<tr>` e filhos `<td>` ou `<th>` tiverem a *mesma* `border-width`, as linhas n&atilde;o ter&atilde;o suas bordas aplicadas. [Veja este JS Bin para um exemplo](http://jsbin.com/yabek/2/).


<a name="buttons-firefox"></a>
### Firefox e bot&otilde;es `<input>`
Por raz&otilde;es desconhecidas, Firefox continua aplicando estilos para `<input>`s dos tipos `submit` e `button` que n&atilde;o podem ser sobrescritos via CSS personalizado. **Prefira elementos `<button>`.**

```html
<!-- Não tão bom -->
<input type="submit" value="Save changes">
<input type="button" value="Cancel">

<!-- Superbom em qualquer lugar -->
<button type="submit">Save changes</button>
<button type="button">Cancel</button>
```
Alguns dos estilos do Firefox podem ser sobrescritos com este snippet de CSS:

```css
input::-moz-focus-inner {
  border: 0;
  padding: 0;
}
```

Entretanto, [como David Walsh apontou](http://davidwalsh.name/firefox-buttons), isso n&atilde;o resolve tudo. Apenas use o elemento `<button>`.

**Boas novas!** Parece que [uma corre&ccedil;&atilde;o pra isso](https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43) deve vir com o Firefox 30. S&atilde;o boas nova para nossos futuros eus, mas isso não vai consertar nada em vers&otilde;es antigas.


<a name="buttons-type"></a>
### Sempre especifique um `type` para `<button>`s
O valor padrão &eacute; `submit`, significando que qualquer bot&atilde;o em um formul&aacute;rio pode submeter este formul&aacute;rio. Use `type="button"` para qualquer coisa que n&atilde;o seja submeter o formulário e `type="submit"` para o que o submete.

```html
<button type="submit">Salvar altera&ccedil;&otilde;es</button>
<button type="button">Cancelar</button>
```
Para a&ccedil;&otilde;es que demandam um `<button>` e n&atilde;o est&atilde;o em um formul&aacute;rio, use `type="button"`.

```html
<button class="dismiss" type="button">x</button>
```
**Fato engraçado:** Aparentemente o IE7 n&atilde;o suporta apropriadamente o atributo `value` em `<button>`s. Ao inv&eacute;s de ler o conte&uacute;do do atributo, ele "puxa" a partir do innerHTML (conte&uacute;do entre a tag de abertura e fechamento de `<button>`). Entretanto, isso n&atilde;o precisa ser visto como grande coisa por duas raz&otilde;es: o uso de IE7 est&aacute; caindo cada vez mais e parece bastante incomum especificar ambos, `value` e innerHTML, em `<button>`s.

<a name="ie-selector-limit"></a>
### Limite de seletor do Internet Explorer
Internet Explorer 9 e abaixo podem ter o m&aacute;ximo de 4.096 seletores por folha de estilo. Eles tamb&eacute;m tem um limite de 31 folhas de estilo combinadas e inclus&otilde;es de `<style></style>`. Qualquer coisa acima desse limite &eacute; ignorado pelo navegador. Escolha entre dividir o CSS ou come&ccedil;ar a refatorar. Sugerimos a &uacute;ltima.

Como uma valiosa nota, aqui est&aacute; como os navegadores contam seletores:

```css
/* Um seletor */
.element { }

/* Dois seletores */
.element,
.other-element { }

/* Três seletores */
input[type="text"],
.form-control,
.form-group > input { }
```

<a name="position-explained"></a>
### "position" explicada
Elementos com `position: fixed;` s&atilde;o colocados relativamente &agrave; viewport do navegador. Elementos com `position: absolute;` s&atilde;o colocados relativamente ao elemento-pai mais pr&oacute;ximo que tenha posicionamento diferente de `static` (e.g., `relative`, `absolute`, ou `fixed`).


<a name="position-width"></a>
### "position" e "width"
N&atilde;o especifique `width: 100%;` em um elemento que tenha `position: [absolute|fixed];`, `left` e `right`. O uso de `width: 100%;` &eacute; o mesmo que o uso combinado de `left: 0;` e `right: 0;`. Use um ou outro; n&atilde;o ambos.


<a name="position-transforms"></a>
### Posição "fixed" e transforms
Navegadores quebram `position: fixed;` quando um elemento-pai de um elemento tem `transform` especificado. Usar transforms cria um novo "bloco que cont&eacute;m", efetivamente for&ccedil;ando o elemento-pai a ter `position: relative;` e o elemento fixo a se comportar como `position: absolute;`.

[Veja a demo](http://jsbin.com/yabek/1/) e [leia o post de Eric Meyer sobre o assunto](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).
