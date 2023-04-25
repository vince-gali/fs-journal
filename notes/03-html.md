# HTML

<!-- Day 2 - html structure -->

start w "basic skeleton" 
- !

header contains info necessary for browser

title tag "document" is what shows up in browser tab
link:favicon allows you to change small image within browser tab
-(insert after document title line)

website content to be within body tags
use "nav" within header tag = use for website navigation

use "main" to determine number of "sections"(main content within page)
-(.left and .right) to determine location of "sections" (within first section tag)
assign class to second section to distinguish between rest of page/other section(for example if it is going to be a darker background)

footer

linking css - "link" & type in style.css as href name
-(crtl click to open/create)


~in css~ 
.root (an element everything has access to)
-(within root add colors (for example: --primary-bg: color;))
-root stores elements

body
-(withing body (for example: background-color: var(--primary;)))
-display: flex
-flex direction: column
-min height = 100vh (vh stands for view height)

main
flex-grow: 1 
-(pushes footer to bottom of page)

create class as ".dark" for example
-invert background and text color within class
~ ~

html
-within head and nav create 3 divs (logo,links,sign up)
flex parent element (nav) - to  align in a row
assign class to nav as "d-flex"
assign "align-items-baseline" after class (aligns text on web page)

*ctrl tab jumps to most recent file*
crtl shift w/ arrows jumps to ends of words

create d-flex class in css (known as utility class)
-.d-flex
display: flex; (within d-flex class)
flex wrap: wrap;

create .align-items-baseline and .align-items-center
^ above will align in a row

when adding margins : 0 0 0 0 (top, right, bottom, left) 0's are in clockwise order
-use "rem" instead of px

justify-content-between (in css and html)

div.d-flex.gap span*5
(create 5 spans for each link (for example about, home, services etc))

assign class "active-link" to 'home' span then create .active-link in css w/ color, font weight, text decoration

think of div as a block
span will take up the width of its content

use * box-sizing: border-box when doing 50%/50% split and img gets pushed to bottom of page
-used for calculating border and width

use "a" tag when creating a line of words to take you to another page







