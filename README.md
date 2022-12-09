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

__iFrames__ and __Frames__ are similar. The small difference is that when working on a Windows application, I use terminology __Frame__. When working with web apps, I normally call them __Frames__.


Why do I need to work with Frames? The reason is that if there are any elements present inside the Frame, I cannot directly interact with it. I cannot do any validations on those elements because those element are present inside the Frame. And that Frame is a customized Form, which is displayed from another 3rd party web page.
