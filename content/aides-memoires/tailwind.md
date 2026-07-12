---
title: "Tailwind CSS"
summary: "Mémo pour les équivalances entre les classes Tailwind et les styles CSS classiques"
date: "2026-07-08"
categories: ["mémo"]
tags: ["site web", "framework", "tailwind", "css"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Layout

| Classe | CSS |
| --- | --- |
| `block` | `display: block;` |
| `inline-block` | `display: inline-block;` |
| `inline` | `display: inline;` |
| `flex` | `display: flex;` |
| `inline-flex` | `display: inline-flex;` |
| `grid` | `display: grid;` |
| `hidden` | `display: none;` |
| `static` | `position: static;` |
| `relative` | `position: relative;` |
| `absolute` | `position: absolute;` |
| `fixed` | `position: fixed;` |
| `sticky` | `position: sticky;` |
| `top-0` | `top: 0;` |
| `right-0` | `right: 0;` |
| `bottom-0` | `bottom: 0;` |
| `left-0` | `left: 0;` |
| `inset-0` | `top:0; right:0; bottom:0; left:0;` |
| `z-10` | `z-index: 10;` |
| `float-left` | `float: left;` |
| `float-right` | `float: right;` |
| `clear-both` | `clear: both;` |
| `overflow-auto` | `overflow: auto;` |
| `overflow-hidden` | `overflow: hidden;` |
| `overflow-scroll` | `overflow: scroll;` |
| `overflow-x-auto` | `overflow-x: auto;` |
| `overflow-y-hidden` | `overflow-y: hidden;` |

---

## Flexbox

| Classe | CSS |
| --- | --- |
| `flex-row` | `flex-direction: row;` |
| `flex-row-reverse` | `flex-direction: row-reverse;` |
| `flex-col` | `flex-direction: column;` |
| `flex-col-reverse` | `flex-direction: column-reverse;` |
| `flex-wrap` | `flex-wrap: wrap;` |
| `flex-nowrap` | `flex-wrap: nowrap;` |
| `flex-wrap-reverse` | `flex-wrap: wrap-reverse;` |
| `justify-start` | `justify-content: flex-start;` |
| `justify-center` | `justify-content: center;` |
| `justify-end` | `justify-content: flex-end;` |
| `justify-between` | `justify-content: space-between;` |
| `justify-around` | `justify-content: space-around;` |
| `justify-evenly` | `justify-content: space-evenly;` |
| `items-start` | `align-items: flex-start;` |
| `items-center` | `align-items: center;` |
| `items-end` | `align-items: flex-end;` |
| `items-stretch` | `align-items: stretch;` |
| `items-baseline` | `align-items: baseline;` |
| `content-start` | `align-content: flex-start;` |
| `content-center` | `align-content: center;` |
| `content-between` | `align-content: space-between;` |
| `self-auto` | `align-self: auto;` |
| `self-start` | `align-self: flex-start;` |
| `self-center` | `align-self: center;` |
| `self-end` | `align-self: flex-end;` |
| `self-stretch` | `align-self: stretch;` |
| `flex-1` | `flex: 1 1 0%;` |
| `flex-auto` | `flex: 1 1 auto;` |
| `flex-initial` | `flex: 0 1 auto;` |
| `flex-none` | `flex: none;` |
| `grow` | `flex-grow: 1;` |
| `grow-0` | `flex-grow: 0;` |
| `shrink` | `flex-shrink: 1;` |
| `shrink-0` | `flex-shrink: 0;` |
| `order-1` | `order: 1;` |
| `order-first` | `order: -9999;` |
| `order-last` | `order: 9999;` |

---

## Grid

| Classe | CSS |
| --- | --- |
| `grid-cols-3` | `grid-template-columns: repeat(3, minmax(0, 1fr));` |
| `grid-rows-3` | `grid-template-rows: repeat(3, minmax(0, 1fr));` |
| `col-span-2` | `grid-column: span 2 / span 2;` |
| `col-start-1` | `grid-column-start: 1;` |
| `col-end-3` | `grid-column-end: 3;` |
| `row-span-2` | `grid-row: span 2 / span 2;` |
| `gap-4` | `gap: 1rem;` |
| `gap-x-4` | `column-gap: 1rem;` |
| `gap-y-4` | `row-gap: 1rem;` |

---

## Espacement

| Classe | CSS |
| --- | --- |
| `p-4` | `padding: 1rem;` |
| `px-4` | `padding-left: 1rem; padding-right: 1rem;` |
| `py-4` | `padding-top: 1rem; padding-bottom: 1rem;` |
| `pt-4` | `padding-top: 1rem;` |
| `pr-4` | `padding-right: 1rem;` |
| `pb-4` | `padding-bottom: 1rem;` |
| `pl-4` | `padding-left: 1rem;` |
| `m-4` | `margin: 1rem;` |
| `mx-4` | `margin-left: 1rem; margin-right: 1rem;` |
| `my-4` | `margin-top: 1rem; margin-bottom: 1rem;` |
| `mt-4` | `margin-top: 1rem;` |
| `mr-4` | `margin-right: 1rem;` |
| `mb-4` | `margin-bottom: 1rem;` |
| `ml-4` | `margin-left: 1rem;` |
| `mx-auto` | `margin-left: auto; margin-right: auto;` |
| `space-x-4 > * + *` | `margin-left: 1rem;` |
| `space-y-4 > * + *` | `margin-top: 1rem;` |

Échelle numérique (`n` dans `p-n`, `m-n`, `gap-n`…) : `1=0.25rem, 2=0.5rem, 3=0.75rem, 4=1rem, 5=1.25rem, 6=1.5rem, 8=2rem, 10=2.5rem, 12=3rem, 16=4rem, 20=5rem, 24=6rem`.

---

## Dimensions

| Classe | CSS |
| --- | --- |
| `w-4` | `width: 1rem;` |
| `w-1/2` | `width: 50%;` |
| `w-1/3` | `width: 33.333333%;` |
| `w-full` | `width: 100%;` |
| `w-screen` | `width: 100vw;` |
| `w-auto` | `width: auto;` |
| `w-fit` | `width: fit-content;` |
| `h-4` | `height: 1rem;` |
| `h-full` | `height: 100%;` |
| `h-screen` | `height: 100vh;` |
| `min-w-0` | `min-width: 0px;` |
| `min-w-full` | `min-width: 100%;` |
| `max-w-md` | `max-width: 28rem;` |
| `max-w-xl` | `max-width: 36rem;` |
| `max-w-none` | `max-width: none;` |
| `min-h-screen` | `min-height: 100vh;` |

---

## Typographie

| Classe | CSS |
| --- | --- |
| `text-sm` | `font-size: 0.875rem; line-height: 1.25rem;` |
| `text-base` | `font-size: 1rem; line-height: 1.5rem;` |
| `text-lg` | `font-size: 1.125rem; line-height: 1.75rem;` |
| `text-xl` | `font-size: 1.25rem; line-height: 1.75rem;` |
| `text-2xl` | `font-size: 1.5rem; line-height: 2rem;` |
| `font-thin` | `font-weight: 100;` |
| `font-normal` | `font-weight: 400;` |
| `font-medium` | `font-weight: 500;` |
| `font-semibold` | `font-weight: 600;` |
| `font-bold` | `font-weight: 700;` |
| `font-black` | `font-weight: 900;` |
| `italic` | `font-style: italic;` |
| `not-italic` | `font-style: normal;` |
| `text-left` | `text-align: left;` |
| `text-center` | `text-align: center;` |
| `text-right` | `text-align: right;` |
| `text-justify` | `text-align: justify;` |
| `leading-none` | `line-height: 1;` |
| `leading-tight` | `line-height: 1.25;` |
| `leading-normal` | `line-height: 1.5;` |
| `leading-loose` | `line-height: 2;` |
| `tracking-tight` | `letter-spacing: -0.025em;` |
| `tracking-wide` | `letter-spacing: 0.025em;` |
| `uppercase` | `text-transform: uppercase;` |
| `lowercase` | `text-transform: lowercase;` |
| `capitalize` | `text-transform: capitalize;` |
| `normal-case` | `text-transform: none;` |
| `underline` | `text-decoration-line: underline;` |
| `line-through` | `text-decoration-line: line-through;` |
| `no-underline` | `text-decoration-line: none;` |
| `truncate` | `overflow: hidden; text-overflow: ellipsis; white-space: nowrap;` |
| `whitespace-nowrap` | `white-space: nowrap;` |
| `whitespace-pre` | `white-space: pre;` |
| `break-words` | `overflow-wrap: break-word;` |
| `break-all` | `word-break: break-all;` |

---

## Couleurs

| Classe | CSS |
| --- | --- |
| `text-blue-500` | `color: #3b82f6;` |
| `bg-blue-500` | `background-color: #3b82f6;` |
| `border-blue-500` | `border-color: #3b82f6;` |
| `opacity-50` | `opacity: 0.5;` |

Format général : `text-{couleur}-{50..950}` / `bg-{couleur}-{50..950}` / `border-{couleur}-{50..950}` → chaque palier a un code hex fixe (ex. `500` = teinte de base, `50` = très clair, `950` = très foncé).

---

## Bordures

| Classe | CSS |
| --- | --- |
| `border` | `border-width: 1px; border-style: solid;` |
| `border-2` | `border-width: 2px;` |
| `border-t` | `border-top-width: 1px;` |
| `border-b` | `border-bottom-width: 1px;` |
| `rounded` | `border-radius: 0.25rem;` |
| `rounded-md` | `border-radius: 0.375rem;` |
| `rounded-lg` | `border-radius: 0.5rem;` |
| `rounded-full` | `border-radius: 9999px;` |
| `rounded-none` | `border-radius: 0px;` |
| `rounded-t-lg` | `border-top-left-radius: 0.5rem; border-top-right-radius: 0.5rem;` |
| `border-solid` | `border-style: solid;` |
| `border-dashed` | `border-style: dashed;` |
| `border-dotted` | `border-style: dotted;` |

---

## Effets

| Classe | CSS |
| --- | --- |
| `shadow-sm` | `box-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);` |
| `shadow-md` | `box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);` |
| `shadow-xl` | `box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);` |
| `shadow-none` | `box-shadow: 0 0 #0000;` |
| `opacity-0` | `opacity: 0;` |
| `mix-blend-multiply` | `mix-blend-mode: multiply;` |

---

## Filtres

| Classe | CSS |
| --- | --- |
| `blur-sm` | `filter: blur(4px);` |
| `blur-md` | `filter: blur(12px);` |
| `brightness-50` | `filter: brightness(0.5);` |
| `contrast-125` | `filter: contrast(1.25);` |
| `grayscale` | `filter: grayscale(100%);` |
| `grayscale-0` | `filter: grayscale(0);` |

---

## Transitions/Animations

| Classe | CSS |
| --- | --- |
| `transition` | `transition-property: color, background-color, border-color, ...; transition-duration: 150ms;` |
| `transition-colors` | `transition-property: color, background-color, border-color;` |
| `transition-opacity` | `transition-property: opacity;` |
| `transition-transform` | `transition-property: transform;` |
| `duration-300` | `transition-duration: 300ms;` |
| `ease-linear` | `transition-timing-function: linear;` |
| `ease-in` | `transition-timing-function: cubic-bezier(0.4, 0, 1, 1);` |
| `ease-out` | `transition-timing-function: cubic-bezier(0, 0, 0.2, 1);` |
| `ease-in-out` | `transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);` |
| `delay-150` | `transition-delay: 150ms;` |
| `animate-spin` | `animation: spin 1s linear infinite;` |
| `animate-ping` | `animation: ping 1s cubic-bezier(0,0,0.2,1) infinite;` |
| `animate-pulse` | `animation: pulse 2s cubic-bezier(0.4,0,0.6,1) infinite;` |
| `animate-bounce` | `animation: bounce 1s infinite;` |

---

## Transform

| Classe | CSS |
| --- | --- |
| `scale-95` | `transform: scale(0.95);` |
| `scale-x-95` | `transform: scaleX(0.95);` |
| `rotate-45` | `transform: rotate(45deg);` |
| `-rotate-45` | `transform: rotate(-45deg);` |
| `translate-x-4` | `transform: translateX(1rem);` |
| `translate-y-4` | `transform: translateY(1rem);` |
| `skew-x-6` | `transform: skewX(6deg);` |

Toutes les classes `transform` cumulent en une seule propriété `transform` via variables CSS internes.

---

## Interactivité

| Classe | CSS |
| --- | --- |
| `cursor-pointer` | `cursor: pointer;` |
| `cursor-not-allowed` | `cursor: not-allowed;` |
| `cursor-wait` | `cursor: wait;` |
| `select-none` | `user-select: none;` |
| `select-text` | `user-select: text;` |
| `select-all` | `user-select: all;` |
| `pointer-events-none` | `pointer-events: none;` |
| `pointer-events-auto` | `pointer-events: auto;` |
| `resize` | `resize: both;` |
| `resize-none` | `resize: none;` |
| `resize-x` | `resize: horizontal;` |
| `resize-y` | `resize: vertical;` |

---

## États (préfixes)

| Préfixe | Équivalent CSS |
| --- | --- |
| `hover:` | `:hover` |
| `focus:` | `:focus` |
| `active:` | `:active` |
| `disabled:` | `:disabled` |
| `visited:` | `:visited` |
| `checked:` | `:checked` |
| `first:` | `:first-child` |
| `last:` | `:last-child` |
| `odd:` | `:nth-child(odd)` |
| `even:` | `:nth-child(even)` |
| `group-hover:` | `.group:hover &` |
| `peer-focus:` | `.peer:focus ~ &` |
| `dark:` | `@media (prefers-color-scheme: dark)` |

---

## Responsive (préfixes de breakpoint)

| Préfixe | Équivalent CSS |
| --- | --- |
| `sm:` | `@media (min-width: 640px)` |
| `md:` | `@media (min-width: 768px)` |
| `lg:` | `@media (min-width: 1024px)` |
| `xl:` | `@media (min-width: 1280px)` |
| `2xl:` | `@media (min-width: 1536px)` |

Mobile-first : classe sans préfixe = tous écrans, préfixe = à partir de cette largeur.

---

## Valeurs arbitraires

| Classe | CSS |
| --- | --- |
| `w-[137px]` | `width: 137px;` |
| `bg-[#1da1f2]` | `background-color: #1da1f2;` |
| `top-[calc(100%-1rem)]` | `top: calc(100% - 1rem);` |
| `grid-cols-[repeat(auto-fill,minmax(200px,1fr))]` | `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));` |
