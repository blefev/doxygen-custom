# doxygen-custom
Modified Dooxygen to match your organization.

# Customizing Doxygen
Doxygen provides a handful of ways to [customize the output](http://www.stack.nl/~dimitri/doxygen/manual/customize.html). The simplest way is to customize the HTML output.

Doxygen allows you to customize the HTML output by modifying a master HTML header, footer and stylesheet. You can then include additional stylesheets and javascript files. The following command will generate the default Doxygen files.

`doxygen -w html header.html footer.html customdoxygen.css`

# How to Integrate

To integrate this into your own project tell your doxyfile to use these 4 files in the HTML section (see the example site for an example of each file), or set these attributes in doxywizard.

* HTML_HEADER=header.html
* HTML_FOOTER=footer.html
* HTML\_EXTRA_STYLESHEET=customdoxygen.css
* HTML\_EXTRA_FILES=doxy-boot.js

# Matching Doxygen's Output to Your Organization
To match your Doxygen output with your organization, you will make heavy use of your browser's developer tools.

In this example, we will be matching the output to South Dakota School of Mine's branding. Branding information for SDSMT can be found [here](http://www.sdsmt.edu/Campus-Services/University-Relations-and-Media/Logos/). The branding guidelines detail the colors used and provide logos. The guidelines mention the logo should not be stretched or skewed, and all original parts must be intact. We can download a logo from the branding page, for example "White on Transparent" to use in our project. Note: Doxygen doesn't resize images, so we can resize it with a program like [imagemagick](https://www.imagemagick.org/script/index.php). To resize the logo using imagemagick to be 64px tall, without affecting the aspect ratio, run `convert -geometry x64 <image-in> <image-out>`.

Next, we need to do some digging with Web Inspector. In Chromium/Chrome/Firefox, the developer console can be brought up with `F12`, or the inspector with `Ctrl+Shift+C`.

Go to your organization's website, and pull up the inspector. Hover your mouse over the element you would like to see the style for and click it. Under the right hand side of the developer tools, you should see a list of CSS rules associated with the element. Crossed out rules generally mean that the rule was over-written by another. 

On [SDSMT.edu](https://sdsmt.edu), pull up the inspector and hover over one of the buttons on the navbar, for example "Apply". Go to Styles in Chromium/Chrome, or Rules in Firefox and you will see a rule for "a:active", with the color of the element defined as `color: #c1a868;`. This is the color we will later use for text on our Doxygen menubar. So store this color for future use.

We can repeat this process for other elements to discover what styles are being used. We can also select elements under the "Elements" tab under Chromium/Chrome, or the left main tab in Firefox dev tools. If we find the `container-fluid backgroundBlue` div under the `contentHeader` div, we will see a style class `backgroundBlue`. This contains the background color for the navbar, which we will also use in our menubar. Store it. 

We can also look for a `font-family`. In Chromium/Chrome, select the `Computed` tab to see what the actual rendered font is. Unfortunately, we can't use all of the fonts on the site because some of them are proprietary, paid-for fonts (e.g. Rockwell). If we want to use a custom font, it either needs to be on the client's computer, or included in the Doxygen output directory. The navbar uses 'pt_sans', which a quick web search will bring up many sites to [download it from](https://www.fontsquirrel.com/fonts/pt-sans). It can then be sourced with:
`@font-face {
  font-family: pt-sans;
  src: url('<YOUR FONT LOCATION>')
}`

If you are unsure that a rule selected corresponds to the element you are looking at, you can edit it in the developer tools, perhaps by changing the `color`. If it changes to the new color, you know it's the correct rule. We will use these steps a lot to figure out what rules need to be changed in Doxygen later.

An alternative way of finding the styles is looking directly at the stylesheets for the website. On [SDSMT.edu](https://sdsmt.edu), press `Ctrl+U` or right click -> View Source to see the page source. We will see numerous stylesheets included for Bootstrap, Javascript Libraries, JQuery, and more. If we continue looking, we'll notice a stylesheet `/css/mines.css`. [Click the link](http://www.sdsmt.edu/css/mines.css) to follow it. This stylesheet contains most of the rules that define the custom colors and fonts of the site. 

Now that we know how to find CSS style rules, we can move on to finding out what elements we want to change in Doxygen. Make a default Doxygen run, or find an example site like the [documentation for D-Bus](https://dbus.freedesktop.org/doc/api/html/index.html) (making sure it's the same Doxygen version), to inspect. Now, we can repeat the process of inspection, but this time we'll want to tinker more to see how our CSS changes might look. We can change the color, font, size, or any number of rules. We should see live updates of the page when we make changes.

One of the first things we might want to change is the title area. Bring up web inspector and hover over it, making sure the inspector shows the `titlearea` element. Let's try changing the background color. In the styles/rules tab of developer tools, click beneath one of the existing rules to create a new one. Try adding "background-color: #011c4f". If it was entered correctly, you should see the title area change to blue. But it doesn't look very good with a black title. Open the inspector again and select the projectname. In the #projectname rules, try adding "color: #c1a868", and you should see it change to the yellow from SDSMT.edu. That's a pretty good start to our project! 

In order to add these changes to Doxygen, we can redefine the rules in our `customdoxygen.css` file. In CSS, elements are just plain words like `b`,`p`,`body`, etc. Classes start with a dot `.` like `.sm-dox`. ID's start with a pound `#`. In the HTML, elements are just normal tags, classes are defined in the tag with `class="classname"`, and ID's are defined like `id="idname"`.

I recommend searching for the element/id/class first, and adding the rule if you find it. Otherwise, it can be a good idea to append it to the file just in case it is defined elsewhere, as CSS is read from top to bottom and rules on the bottom overwrite rules above. 

