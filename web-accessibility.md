# Web Accessibility

---

# Bevezetés

---

* [Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG21/)

### Négy alapelv:
* perceivable (felfogható): az információknak és a felhasználói felület komponenseinek, a felhaszáló számára érzékelhető módon kell megjelenniük
* operable (működtethető): a felhasználói felület összetevőinek és a navigációnak működőképesnek kell lennie
* understandable (érthető): az információknak és a felhasználói felület komponenseinek érthetőeknek kell lenniük
* robust (robusztus): szabványos kód használata

### Irányelvek
* Az iránymutatások megadják azokat az alapvető célokat, amelyekre törekedni kell annak érdekében, hogy a tartalmat hozzáférhetőbbé tegyék a különböző fogyatékossággal élő felhasználók számára.

### Sikerkritériumok
*  Lehetővé teszik a WCAG használatát ott, ahol a követelmények és a megfelelőségi vizsgálatok szükségesek.
*  Három megfelelőségi szintet határoznak meg: A, AA és AAA.

---

# Az akadálymentesítés legfontosabb szempontjai

### Billentyűzettel legyen hozzáférhető a navigáció, funkciók, tartalom

* A kontent minden eleme billentyűzeten keresztül elérhető (Tab, Shift + Tab, nyilak)
* Ha elérhető egy elem, akkor az el is hagyható
* Minden funkció kiválasztható (Enter, Space)
* A fókusz látható

* tabindex: meghatározza, hogy egy elem fókuszálható-e, illetve a tabsorrendet
* Alapértelmezett sorrend a DOM-ban való elhelyezkedés

```html
Alapvetően tabozható elem kivétele a tabsorrendből:
<button tabindex="-1"/>NEM TABOZHATÓ</button>

Egy alapvetően nem tabozható elem tabozhatóvá tétele:
<div tabindex="0"></div>

Tabsorrend meghatározás:
<button tabindex="3">A</button>
<button tabindex="2">B</button>
<button tabindex="1">C</button>
<button tabindex="2">D</button>
```

* Fókusz mozgatása

```html
<button id="navigationExampleButtonA" tabindex="3" onkeydown="focusHandling(event, this)">A</button>
<button id="navigationExampleButtonB" tabindex="2">B</button>
<button id="navigationExampleButtonC" tabindex="1" onkeydown="focusHandling(event, this)">C</button>
<button id="navigationExampleButtonD" tabindex="2">D</button>
<script>
function focusHandling(event, element) {
    if(element.id === "navigationExampleButtonA") {
        if(!event.shiftKey && event.key === "Tab") {
        	event.preventDefault();
            document.getElementById("navigationExampleButtonC").focus();
        }
    } else {
        if(event.shiftKey && event.key === "Tab") {
        	event.preventDefault();
            document.getElementById("navigationExampleButtonA").focus();
        }
    }
}
</script>
```

### A fókusz logikus sorrendben mozogjon

* A tabsorrend a következő sorrendet kövesse:
    * balról jobbra
    * föntről lefele
    * fejléc az első aztán navigáció (ha van), lapok köztni navigáció (ha van), végül a lábléc

### Az űrlapelemek logikus laprendben legyenek rendezve és címkézve

```html
<form>
  <label for="username">Username:</label><br>
  <input type="text" id="username" name="username"><br>
  <label for="password">Password:</label><br>
  <input type="password" id="password" name="password"><br>
  <input type="submit" value="Submit">
</form> 
```

### A címkék a mezőkhöz, checkboxokhoz illetve radiogomokhoz vannak kötve, ezek megváltoztatásukról visszajelzést kap a felhasználó
<br>

### A linkszövegek elmagyarázzák a link célját
* Kerülendő: Click Here, More
* Helyette: link to... 

### Minden nem szöveges elemhez alternatív szövegek (alt attribútumok) álljanak rendelkezésre

```html
Kép alternatív szöveggel:
<img src="dog.png" alt="Rottweiler fut egy mezőn, háttérben hegyek">
```

### Az oldalak úgy legyenek felépítve, hogy a társított style sheet nélkül is olvashatóak maradjanak
<br>

### Ha a színek mint információ hordozó vannak használva akkor ne csak ezek hordozzák az információt
<br>

### Elég nagy a kontraszt ahhoz, hogy a látássérült felhasználóknak olvasható és felismerhető legyen
<br>

### A böngésző nagyítása és a betűméret megváltoztatása támogatott
<br>

### Reszponzív oldal
<br>

### HTML-fejlécelemek (h1, h2, h3 stb.) használata a vizuális címsorok megjelenítésére. A HTML címsor elemeinek hierarchiája megfelel a tartalom megjelenésének
<br>

### A navigációs elemek, különösen a menük, mindig rendezett listaként vannak formázva
<br>
---

# ARIA (Accessible Rich Internet Applications)

---

* [Accessible Rich Internet Applications (ARIA)](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

* Attribútumok halmaza, melyek meghatározzák a kisegítő programok számára webes tartalom természetét.


### ARIA roles és ARIA attribútumok

* button
* aria-pressed: toggle button
```html
<div tabindex="0" role="button" aria-pressed="false">Save</div>
<script>
```

* checkbox
* aria-checked
* aria-labelledby
```html
<label id="checkboxLabel">Cheeckbox:</label>
<div role="checkbox" aria-checked="false" tabindex="0" aria-labelledby="checkboxLabel"></div>
```

* form
```html
<div role="form" id="contact-info">
  űrlap kontent pl: inputok
</div>
```

* heading
* aria-level
```html
<div role="heading" aria-level="1">H1 heading</div>
```

* img
* több képet egy képként való értelmezésre
```html
<div role="img">
  <img src="img1.png">
  <img src="img2.png">
</div>
```

* list és listitem
```html
<div role="list">
  <div role="listitem">List item 1</div>
  <div role="listitem">List item 2</div>
  <div role="listitem">List item 3</div>
</div>
```

* radiogroup és radio
```html
<div role="radiogroup">
  <div role="radio" aria-checked="false"></div>
  <div role="radio" aria-checked="false"></div>
  <div role="radio" aria-checked="true"></div>
</div>
```

* arialabel
```html
<img src="dog.png" arialabel="Rottweiler fut egy mezőn, háttérben hegyek" >
```
