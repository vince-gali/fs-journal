# Bootstrap

day 3 bootstrap

css-tricks.com (resource)

rows are horizontal
columns are spaces within rows
col-12 takes up full page
col -6 & -6 splits columns 50/50 within row
each row essentially has 12 columns
no rows within rows
rows inside of containers (containers are essentially header,main,footer)
columns inside of rows 

container class provides extra margin on sides
container-fluid aligns along margins

<!-- starting html using bootstrap -->
bootstrap link inserted before style.css link (use cdn css link from bootstrap site)

assign container-fluid class to header
(within header) .row 'tab'
.col-6 (column example)

within main - assign number of rows

for footer use .row then add div below w col-12

change div to section for each row (section class = "row")

use section tag instead of div when using rows

on medium screens and larger change from col-12 to col-4 (div class = col-12 col-md-4)

assign img class = "img-fluid' to img class to allow the image from spilling past page

when two diff text columns are side by side in browser but in mobile use:
        div class ="col-12 col-md-6"
        p tag with text 
        (for each text)

mx- to apply margins within p tag (for example) along x axis
mb- applies margin to bottom

use sticky-class within header class to keep header fixed while scrolling

use text-center within parent class to align center

to add spacing within columns (making pictures not touch elements above/below) use padding
(within parent class use pt(padding top) pe(padding end) etc) px on sides / use p only to add padding all around img

add border radius within img class img class = "img-fluid rounded"

fw-bold within tag 

text center within button class centers text within button
text center within div class centers button on page

use mdi(material design icons) for icons (social icons)
link mdi below bootstrap link

use i tag to include icon and for class use mdi mdi-(whatever icon you want)
use fs to change size of icons within i tag (1 being largest, 5 being the smallest)

when trying to change order of footing when in mobile use order-0
