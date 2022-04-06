# Bootstrap4のmdで改行

```css
@media screen and (max-width: 767.98px) {
    .br::before {
        content: "\A" ;
        white-space: pre ;
    }
}
```