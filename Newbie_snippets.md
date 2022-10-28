## Intro - 
(file Written by Lazza)
THIS FILE IS WORK IN PROGRESS AND FOR NOW THE BELOW ARE SNIPPETS I"M ADDING AS I GO SO I DON'T FORGET

I found in my first couple of weeks of getting into Kivy that there's alot of random Kivy examples out there that are written mainly in python 
and far less out there in the actual kv language. So this document is highlighting (in perhaps a clearer newbie manner ?) some of the things that 
took me ages to find or get my head around as a new user with minimal experiance. 

I'm a newbie to Kivy also, therefore this is one newbie helping another. I also find I learn more if I try to teach. Therefore writing it down 
in this manner regardless if anyone reads it helps me to also learn and remember :). If you find something that needs tweaked feel free
to let me know.

I personally think that most people choosing to use Kivy will be those that prefer Python, which is understandable, however as a result those people 
usually write things in python rather than kv, hence the examples and tutorials on the net being mainly in python. 
Kivy have added the kv language for a reason so i'm choosing to teach myself that language 
as well and use it as much as I can. So far I can see the benifits to using the kivy language. it's quick, it's often less code than what it would be
writing the same thing in python. Unfortunately I find even the kivy documentation can sometimes focus more on the python code 
than the actual kv language which is weird. Also their documentation I find often has VERY limited examples

# Table of Contents

- [The KV file](#The-kv-file)
- [Dynamic Classes](#Adding-What-Kivy-call-a-Dynamic-Class)
- [Drop Down example](#Dropdown-example)
- [Drop down Menu](#Drop-down-menu)
- [Size hint explanantion](#Explanation-from-stackexchange-about-Size-hint-that-i-found-helpful)

# The kv file

Kivy uses a mixture of Python to run the "logic side" of a android app, and the .kv file to do the majority of the visual stuff 
(IE: the GUI - the stuff you visually see on the screen). The kv file so far to me appears to be a nice quick way to structure your GUI.
As much as i'm aiming to use the kv language as often as practical I have noticed that when it comes to making dynamic widgets your best to do 
that in python, hence another reason alot of examples on the net are probably in python *shrugs*. Anyway... the purpose of this document I'm 
aiming to show more kv examples than python. Give it a go, I reckon you'll grow to like it 



# Adding What Kivy call a Dynamic Class
A dynamic class is like a CSS file. You can make your own dynamic class up so that you don't have to repeat the same thing in your code over and over again.
As a bonus if you need to change the font size of all your labels for example, you can change just the dynamic classes font_size: and it will effect all 
labels in your kv file that use that dynamic class.

Let's take a look at an example below:
```
<BlueLabel@MDLabel>: # <--- This says.... When you put a BlueLabel: property in the kv file, treat it as a MDLabel BUT auto insert the properties listed below it
    font_size: dp(15)
    text_size: self.size
    valign: "center"
    size_hint_y: None
    height: dp(35)
    theme_text_color: "Custom"
    text_color: 0, 0, 1, 1
```
\<Bluelabel@MDLabel>: is the dynamic class inheritance [Kivys documentation on Dynamic classes](https://kivy.org/doc/stable/api-kivy.lang.html#dynamic-classes)
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

# Dropdown example

```
Button:
    id: click_me_button
    text: "Click Me"                                        
    on_parent: resident_drop_content.dismiss()
    on_release: root.select_residency()
DropDown:
    id: resident_drop_content
    on_select: residency_status.text = f'{args[1]}' # see note below
Button:
    id: residency_btn1
    text: 'Resident'
    size_hint: None, None
    height: 35
    width: 250
    background_color: 0,0,1,.3
    on_release: resident_drop_content.select('Resident')
Button:
    id: residency_btn2
    text: 'Visa Holder'
    size_hint: None, None
    height: 35
    width: 250
    background_color: 0,0,1,.3
    on_release: resident_drop_content.select('Visa Holder')
```

# Drop down menu


Did you want to add a drop menu to your app?

If so you can do that as per below. This will make a drop menu appear when a user clicks a button. From there the user can then
select a item from the drop list

in your python file add...
```commandline
def add_drop_down(self, values, caller_id, callback):
        """
        Add a drop menu to the clicked button.
        :param values: Is a list of values to show in the drop menu 
        :param caller_id: This is the "id" of the button tht was clicked to initiate the drop menu
        :param callback: name of the callback function this will call
        :return: 
        """
        # NOTE: replace "className" with the name of the class this code is placed in
        func = getattr(classname, f"{callback}")

        menu_items = [
            {
                "text": f"{i}",
                "viewclass": "OneLineListItem",
                "on_release": lambda x=f"{i}": func(self,x),
            } for i in values
        ]
        
        mymenu = MDDropdownMenu(
            caller=getattr(self,caller_id),
            items=menu_items,
            width_mult=4,
        )
        mymenu.open()
```
Example of usage...
```commandline
    def yes_no_responce(self):
        self.add_drop_down(values=["yes","no","maybe","perhaps"] caller_id="yes_no_responce_id", callback="callback_for_yes_no_responce")

    def callback_for_yes_no_responce(self, result):
        print(result)

```
and in the .kv file
```commandline
Button:
    id: yes_no_responce_id
    text: "Click to choose yes or no"
    on_release: root.yes_no_responce()
```


# Explanation from stackexchange about Size hint that i found helpful

In kivy most of widgets have an argument called size_hint which as the name suggests sets size of widget according
to screen size. It takes value from 0 to 1. A value of 0.5 basically means half of screen. 
You can use size_hint_x and size_hint_y to define size along x and y axis or together you can use size_hint
= (0.5,0.5). 
Also, if in some cases when you couldn't use size_hint then you can you Window.size to get the screen size of
the device as a tuple.
Assume you want to make something with size 50% of both x and y. 
Then you can set ```size = (Window.size[0]*0.5, Windows.size[1]*0.5) ```

Window.size[0] is basically length along x-axis 

and Window.size[1] along y axis