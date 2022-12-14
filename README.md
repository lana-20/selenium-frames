# Frames / iFrames


Example: https://www.selenium.dev/selenium/docs/api/java/index.html?overview-summary.html

![image](https://user-images.githubusercontent.com/70295997/206797760-48d1363a-973b-4db6-abb1-83bcf0a3f6f1.png)

Sometimes a web page is divided into multiple sections called _Frames_. I can see one web page inside of another. Using a living room analogy, the room is the web page, while the TV (screen) is a frame embedded inside the room.

In Selenium 3, I used the switch_to_frame() method. In Selenium 4, I use the following methods:

	switch_to.frame(name_of_the_frame)

	switch_to.frame(id_of_the_frame)

	# If ID or Name are N/A. Frame itself is a web element.
	switch_to.frame(webelement)

	# If only one single frame present on page. Don't try with multiple frames - no index seen anywhere.
	switch_to.frame(0)

	switch_to.default_content()
	
Terms like _frame_, _iframe_, _form_, _inner frame_ are used.

__iFrames__ and __Frames__ are similar. The small difference is that when working on a Windows application, I use terminology __Frame__. When working with web apps, I normally call them __iFrames__.


Why do I need to work with Frames? The reason is that if there are any elements present inside the Frame, I cannot directly interact with it. I cannot do any validations on those elements because those element are present inside the Frame. And that Frame is a customized Form, which is displayed from another 3rd party web page.

To be able to interact with the elements that are present inside a Frame/iFrame, I need to _.switch_to()_ that particular frame first. In my Example, I have 3 different frames. Although the frames in a web page are distinctly different, internally they are integrated/connected.

I have one page that is split into 3 sections/iFrames:

1) Frame # 1 - Packages
2) Frame # 2 - All Classes
3) Frame # 3 - Links & Info

_Requirement:_ Perform operations on the frame. Click on the 1st link in the Packages Frame. Then click on the 'WebDriver' link from the 2nd Frame. Finally, click on the 'HELP' menu in the 3rd Frame.

I cannot directly perfrom these activities. There are 3 elements I need to interact with. The 1st element is a link in the Packages Frame. The 2nd element is the 'WebDriver'link in the 2nd Frame. The 3rd element is the HELP menu, which is present inside the 3rd Frame. These 3 elements are present in 3 different frames. I cannot directly interact with these elements. Before that, I need to switch to that particular frame, perform an action, go to the next frame, do an action, and then switch to the 3rd and last frame and perform an action in it.

Need to switch from one frame to another. If I try to perfom an action directly, I get the __NoSuchElementException__ error.

As soon as I open the web page, the driver is focused on the entire page. Now I want to interact with the 1st element present in the 1st frame. In order to perform any activity in the a frame, I need to switch to it. I use the Selenium 4 command _.switch_to.frame()_:

	driver.switch_to.frame(" ")

I specify the the Name or ID of the frame, or pass the frame as a web element.

First of all, I check if that particular frame has the _name_ attribute or not. I can use HTML tagnames _frame_, _iframe_, or _form_. If the frame tag contains the _name_ attribute, I don'e even need a locator. I can directly specify that _name_ in double quotes inside the _.switch_to_ method. No locator or _.find_element()_ method needed.

<img src="https://user-images.githubusercontent.com/70295997/206818924-e2385fee-5f00-40ad-a970-facfe4b65881.png" width=600></img>

Switch to the 1st frame:

	driver.swtich_to.frame("packageListFrame")
	
And then perform the _.click()_ action on the element wihin the 1st frame:

	driver.find_element(By.LINK_TEXT, "org.openqa.selenium").click()

I repeat the steps for the next 2 commands. Before performing the next action, I switch to the 2nd frame:

	driver.switch_to.frame("packageFrame")

And then perform the action:

	driver.find_element(By.LINK_TEXT, "WebDriver").click()
	
Similarily, with the 3rd action, perform action _after_ switching to the 3rd frame:

	driver.switch_to.frame("classFrame")
	driver.find_element(By.XPATH, "abc").click()
	
To switch the driver focus from the Frame to the Main Page, switch to default content:

	driver.switch_to.default_content()

Otherwise, I get the _NoSuchElementException_ error message. I don't get any errors related to the 1st frame. I successfully switch to the 1st frame and perform the action. I get this error on the 2nd frame, if I don't switch the driver focus from the 1st frame to default content (main page). After the last action in the last (3rd) frame, there's no need to switch back to default content.

<img src="https://user-images.githubusercontent.com/70295997/206811609-94b60a84-7d22-474c-a428-5dcdbaa24728.png" width=600></img>

The Example exhibits 3 Frames that are completely independent. Default content is required because there are multiple frames to navigate to.

__Inner Frame__

Sometimes, I run into an __Inner Frame__, which is frame within a frame. And that frame contains elements. I switch to the Outer Frame, then switch to the Inner Frame, and then interact with an element. Here, I don't have to deal with the default content. 

Example: https://demo.automationtesting.in/Frames.html

<img src="https://user-images.githubusercontent.com/70295997/206813918-0a14eb7b-f308-421d-93b7-5db90c601851.png" width=600></img>

<img src="https://user-images.githubusercontent.com/70295997/206811666-f8e01b0e-f0ae-4256-8bb9-16fc9525fc2c.png" width=600></img>

When inspecting the page, I select the _iframe_ tag for the Outer Frame. The tag element doesn't have a Name or ID. Here, I use the web-element-flavor of the _switch_to_ command. I pass a _web element_ into the _.switch_to.frame()_ function (after identifying the element).

Identify the Outer Frame as a Web Element (when the frame has no ID or Name):
1. From the browser DevTools view the HTML code.
2. Right-click on the _iframe_ element > Copy Xpath.
3. Retain that frame as the _outer_frame_ web element.
4. Pass it to the _switch_to_ command as a web element.


After switching the driver focus to the Outer/Parent Frame, I need to go to the Inner/Chld Frame. _.default_content()_ is not required, since inner_frame is inside the outer_frame. Switch the driver focus to the Inner Frame with the _switch_to_ command. Then identify the text box element and _.send_keys()_ to it.

I don't have anything inside outer_frame, except for the inner_frame. Now, that I've interacted with the input box element inside the inner_frame, there's no point navigating outward to outer_frame. There's nothing in it. By using _.default_content()_ I can go directly to the Web Page, but not directly to the Outer/Parent Frame.

However, Selenium 4 introduced a command that lets me go directly into the Parent Frame (from the Inner Frame). Suppose the outer_frame has some other inner elements. If I want to perform any actions there, I switch to the Parent Frame:

	driver.switch_to.parent_frame()

No need to pass anything into the _.parent_frame()_ command. It goes directly to the Parent Frame, because there is only one parent element.








	
