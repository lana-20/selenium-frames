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

