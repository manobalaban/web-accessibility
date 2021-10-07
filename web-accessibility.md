# Web Accessibility

* Célj nem csak a különböző fogyatékossággal élő emberek segítése az internet vagy web applikációk használatban, henem irányelveket ad amelyek segítségével mindenki számára könnyebben használható alkalmazások tervezhetőek és készíthetőek.
* Elsődlegesen tervezési folyamatoknál használatos szempontokról szól
* Szükséges fejlesztői tudás a html és a javascript ismerete

### Felosztása az előadásnak

1. Áttekintése a WCAG-nak
2. Legfontosabb szempontok akadálymentesítésnél
3. Leggyakrabban használt fejlesztői megoldások, példák

---

# I. [WCAG (Web Content Accessibility Guidelines)](https://www.w3.org/TR/WCAG21/)

* Akadálymentességi irányelvek a W3C (internetes szabványokért felelős konzorcium) gondozásában

### Négy alapelv:
* perceivable (észlelhető): az információknak és a felhasználói felület komponenseinek, a felhaszáló számára érzékelhető módon kell megjelenniük
* operable (működtethető): a felhasználói felület összetevőinek és a navigációnak működőképesnek kell lennie
* understandable (érthető): az információknak és a felhasználói felület komponenseinek érthetőeknek kell lenniük
* robust (robusztus): szabványos kód használata, ez biztosítja a kompatibilitást más programokkal

### Irányelvek
* Az iránymutatások megadják azokat az alapvető célokat, amelyekre törekedni kell annak érdekében, hogy a tartalmat hozzáférhetőbbé tegyék a különböző fogyatékossággal élő felhasználók számára.

### Sikerkritériumok
*  Lehetővé teszik a WCAG használatát ott, ahol a követelmények és a megfelelőségi vizsgálatok szükségesek.
*  Három megfelelőségi szintet határoznak meg: A, AA és AAA.

---

# II. Az akadálymentesítés legfontosabb szempontjai

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

### Az űrlapelemek logikus rendben legyenek rendezve és címkézve

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

### Minden nem szöveges elemhez alternatív szöveg (alt attribútum) álljon rendelkezésre

```html
Kép alternatív szöveggel:
<img src="dog.png" alt="Rottweiler fut egy mezőn, háttérben hegyek">
```

### Az oldalak úgy legyenek felépítve, hogy a társított style sheet nélkül is olvashatóak maradjanak
<br>

### Ha a színek mint információ hordozó vannak használva akkor ne csak ezek hordozzák az információt

```html
<input id="colorExampleInput" type="text" onblur="onBlur()"><br>
<label id="errorMessage" style="visibility: hidden; color: red">*Kötelező mező</label>

<script>
function onBlur() {
	document.getElementById("colorExampleInput").style.border = "1px solid red";
 	document.getElementById("errorMessage").style.visibility = "visible";
}
</script>
```

### Elég nagy a kontraszt ahhoz, hogy a látássérült felhasználóknak olvasható és felismerhető legyen
* High Contrast Mode (left Alt + left Shift + Print Screen)
<br>

[contrast analyser](https://webaim.org/resources/contrastchecker/)

### A böngésző nagyítása és a betűméret megváltoztatása támogatott
<br>

### HTML-fejlécelemek (h1, h2, h3 stb.) használata a vizuális címsorok megjelenítésére. A HTML címsor elemeinek hierarchiája megfelel a tartalom megjelenésének
<br>

### A navigációs elemek, különösen a menük, mindig rendezett listaként vannak formázva
<br>

---

# III. [ARIA (Accessible Rich Internet Applications)](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

* Attribútumok halmaza, melyek meghatározzák a kisegítő programok számára a webes tartalom és alkalmazások akadálymentesítését.

---
### ARIA roles és ARIA attribútumok

* alert
* dinamikusan változó információ átadására, a felolvasó program azonnal elkezdi felolvasni amint megjelenik az elem
```html
<input id="colorExampleInput" type="text" onblur="onBlur()"><br>
<label id="errorMessage" style="visibility: hidden; color: red" role="alert">*Kötelező mező</label>

<script>
function onBlur() {
	document.getElementById("colorExampleInput").style.border = "1px solid red";
 	document.getElementById("errorMessage").style.visibility = "visible";
}
</script>
```

* button
* aria-pressed: toggle button
```html
<div tabindex="0" role="button" aria-pressed="false">Save</div>
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

* progressbar
```html
<div role="progressbar" aria-valuenow="50" aria-valuemin="0" aria-valuemax="100">50 %</div>
```

* aria-label
```html
<img src="dog.png" aria-label="Rottweiler fut egy mezőn, háttérben hegyek" >
```
---

# IV. Összefoglaló (mit ellenőrizhet a fejlesztő)

* Billenytű használat Tab/Shift+Tab (ne érj az egérhez)
  * Minden funkció működtethető?
  * A fókusz mindeig látható?

* High Contrast Mode bekapcsolásakor minden látható-e?

* Ellenőrizzük contrast analyser használatával a színeket.

* Zoom után is láthatóak-e az elemek?

* Képernyőolvasó a megfelelő információkat adja-e át?

[JAWS felolvasó program](https://support.freedomscientific.com/Downloads/JAWS)
<br>
[JAWS hotkeys](https://www.freedomscientific.com/training/jaws/hotkeys/)
