 # Flex bug in Firefox 110 (no bug in Chrome, Edge or Safari)

```html
<div class="flex">
  <div class="flex__item">
    <img class="image" src="image.jpg" alt="">
  </div>
  <div class="flex__item">
    <img class="image" src="image.jpg" alt="">
  </div>
  <div class="flex__item">
    <img class="image" src="image.jpg" alt="">
  </div>
</div>
```

If **.flex** has *RELATIVE* height and has no parents with absolute height, **.flex__items** don't keep ratio of their images.

If we set for **.flex** *ABSOLUTE* height (e.g. height: 200px) - bug disappears.

The only solution I found is to set *ABSOULTE* height via JS at the beginning
and after each window resize.

```JS
copyElementHeight('.content','.flex');

window.addEventListener('resize', () => copyElementHeight('.content','.flex'));

function copyElementHeight(selectorFrom, selectorTo) {
  const elFrom = document.querySelector(selectorFrom);
  const elTo = document.querySelector(selectorTo);

  if (elFrom && elTo) {
    const elFromComputedStyles = getComputedStyle(elFrom);
    const paddingTop = parseInt(elFromComputedStyles.paddingTop) || 0;
    const paddingBottom = parseInt(elFromComputedStyles.paddingBottom) || 0;
    elTo.style.height = `${elFrom.getBoundingClientRect().height - paddingTop - paddingBottom}px`;
  }
}
```