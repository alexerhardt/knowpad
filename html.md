## Centering rows vertically with flexbox

Centers rows on the middle of the vertical axis.

`
<div class="outer">
    <div class="inner"></div>
    <div class="inner"></div>
</div>

.outer {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    align-content: center;
}

.inner {
    width: 100%;
}
`
