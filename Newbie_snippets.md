## Intro - 
(file Written by Lazza)

I found in my first couple of weeks of getting into Kivy that There's alot of random Kivy examples out there that are written mainly in python 
and far less out there in the actual kv language. So this document is highlighting (in perhaps a clearer newbie manner) some of the things that 
took me ages to find or get my head around. 

I'm a newbie to Kivy also, therefore this is one newbie helping another. I also find i learn more if i try to teach. Therfore writing it down 
in this manner regardless if anyone reads it helps me to also learn and remember :). If you find somethign that needs tweaked feel free to let me know

## The .kv file

Kivy uses a mixture of Python to run the "logic side" of a android app, and the .kv file to do the majority of the visual stuff 
(IE: the GUI - the stuff you visually see on the screen) 

THIS FILE IS WORK IN PROGRESS AND FOR NOW THE BELOW ARE SNIPPETS I"M ADDING AS I GO SO I DONT FORGET 

## Adding What Kivy call a "Dynamic Class"
A dynamic class is like a css file. You can make your own dynamic class up so that you don't have to repeat the same thing in your code over and over again.
As a bonus if you need to change the font size of all your labels for example, you can change just the dynamic classes font_size: and it will effect all 
labels in your kv file that use that dynamic class.

Let's take a look at an example below:
```
<BlueLabel@MDLabel>: # <--- This says.... When you put a BlueLabel: property in the kv file, treat it as a MDLable BUT auto insert the properties listed below it
    font_size: dp(15)
    text_size: self.size
    valign: "center"
    size_hint_y: None
    height: dp(35)
    theme_text_color: "Custom"
    text_color: 0, 0, 1, 1
```
\<Bluelabel@MDLabel>: is the dynamic class inheritance (https://kivy.org/doc/stable/api-kivy.lang.html#dynamic-classes)[Kivys documentation on Dynamic classes]
  - The <BlueLabel part can be what ever name you want to call it, in this case its for creating a blue label :)
  - The @ is required so Kivy knows its going to be a dynamic class
  - MDLabel> is what widget your basing it off, this could be Button>, MDButton> or any other widget name
 
 Under the \<Bluelabel@MDLabel>: we have all the properties we want BlueLabel to use as default, such as font_size:, Height: etc

In your actual kv file code you can now instead of using just MDLabel: then writing every property individually for ever MDLabel, put something like
```
BlueLabel:
    text: "I'm a blue label"
    halign: "left"
 ```   
 Effectively this will get evaluated as 
 ```
 BlueLabel:
    text: "I'm a blue label"
    halign: "left"
    font_size: dp(15)
    text_size: self.size
    valign: "center"
    size_hint_y: None
    height: dp(35)
    theme_text_color: "Custom"
    text_color: 0, 0, 1, 1
```
which is effectively the same as writing
```
MDLabel:
    text: "I'm a blue label"
    halign: "left"
    font_size: dp(15)
    text_size: self.size
    valign: "center"
    size_hint_y: None
    height: dp(35)
    theme_text_color: "Custom"
    text_color: 0, 0, 1, 1
 ```
 But you've done it by setting a dynamic class then everytime you want a blue label you only need to type at minimum
 ```
 BlueLabel:
    text: "I'm a blue label"
 ```
