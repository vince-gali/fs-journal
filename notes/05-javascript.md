# JavaScript

<!-- replit challenge -->
function evenOrOdd(i){
    if (i % 2 ==0)
    return 'even'
}
return 'odd'

OR

return i % 2 == 0? 'even': 'odd'
<!--  -->

-Data types (Below)

 <!-- Strings -->
 " " double quotes
 ' ' single 
 ` ` backticks

 <!-- Numbers -->
  whole, negative numbers and decimals
  - 

  <!-- Boolean -->
  true or false statements

  <!-- the weird ones -->
  undefined
  null
  NaN (Not a Number)

  <!-- objects -->
  denoted by {}

  <!-- Arrays -->
  denoted by []

create variable 
-let heroName = 'Miles'
 
 return alert used to stop a function ()
 same as throw new error

 deal w fail conditions first (referencce instructor code)

 ! exclamation point means not (false)
 



 <!-- JAVASCRIPT -->

 (LOOPS, ARRAYS, DOM)

SHIFT ALT DOWN - copies current line to line below

(lecture using animal murder mystery)

random function
- function selectRandomMurderer(){
   const randomIndex = Math.floor(Math.random() * animals.length)
   murderer = animals[randomIndex]    
}

selectRandomMurderer() <--- to invoke a random murderer when page is opened

to give hints when animal is not the murderer 

div class = "display-3" changes font size

focus on understanding parameters
parameter is what you define when you define a function


<!-- sandwich shop lecture wednesday 5/3 -->

 script tag for js goes at bottom of body
 rows can be nested inside of columns

 ---javascript---

 use array for list of 'sandwiches'
EXAMPLE BELOW:
 const sandwiches = [
  {
    name: 'caprese', (STRING)
    price: 11,        (NUMBER)
    quantity: 0
  },
  {
    name: 'blt',
    price: 10,
    quantity: 0
  }
 ]

 ~CLICK ON IMG (sandwich) & ADD TO CART (FUNCTION)~

function buyCaprese(){
    <!-- i need to know which sandwich i want to buy. so find the caprese -->
    <!-- 'purchase' sandwich, increase the sandwich's quantity -->
    <!-- very first line in function should be a console.log() -->
    console.log('buying caprese')
    <!-- privide onclick to img(BUTTON) of caprese sandwich in html and name button onclick same as function (buyCaprese) -->
    let foundSandwich = sandwiches[0] (0 because caprese position is the first one in the array)
    console.log(foundSandwich)
    <!-- comment out console.logs once you know they work -->
    foundSandwich.quantity++
    console.log(foundSandwich)

}
    DO SAME FOR REST OF 'SANDWICHES'

IF DATA IN ARRAY CHANGES YOU WILL NEED TO REFACTOR 


<!-- FUNCTION BELOW IS THE FIRST TWO FUNCTIONS ABOVE BUT REFACTORED-->

function buySandwich(sandwichName){
  //name onlcick in html as buySandwich() instead of buyCaprese//
  // in html onclick = buySandwich() add name of sandwich in parenthesis with single quote //
  console.log('buying sandwich', sandwichName)
  <!-- i need to know which sandwich i want to buy. so find the caprese -->
     <!-- purchase' sandwich, increase the sandwich's quantity -->
  let foundSandwich = sandwiches.find(sandwich=> sandwich.name == sandwichName)
                                        ^this word isn't connected to anything
  console.log(foundSandwich)
  foundSandwich.quantity++
  <!-- after increasing quantity, show change in my cart -->
  console.log(buying sandwich)
<!-- add debugger under function to pause and go thru code of function in console in real time -->
}


~ADDING ITEMS TO CART ON SITE~

function drawCart(){
  console.log('drawing car')
  <!-- for each sandwich that i purchased, draw it to my cart (check the quantity) -->
  let template = ''
  <!-- use backticks for string interpolation -->
  sandwiches.forEach(s=> s.quantity =>
    if (s.quantity > 0){
      template+= `(add in text from html)` see sam's code as reference
    }
  )
document.getElementById('cart').innerHTML = template
drawTotal()
}


~TO GET TOTAL TO INCREASE WHEN ITEMS ARE ADDED TO CART~
function drawTotal(){
  let total = 0
  sandwiches.forEach(s=> w.quantity>0? total += s.quantity*s.price : total += 0)
  console.log(total, 'total');
  document.getElementById('total').innerText = total.toString()
}


~CHECKOUT BUTTON ~
function checkout(){
  sandwiches.forEach(s=> {
    s.quantity = 0 
  })
  drawCart()

}

~REMOVE ITEM~
function removeItem(sandwichName){
  console.log('remove item', sandwichName)
  let foundSandwich = sandwiches.find(s=> s.name == sandwichName);
  foundSandwich.quantity--
  drawCart()
}


give id where you want template to come into
templates will usually be in draw functions
 do draw function first 
  parameters go into function name 





  <!-- fireflex -->
  function drawKneadyBoy(){
    console.log('drawing kneady boy')
    
  }
  drawKneadyBoy()