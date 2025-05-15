## Uniapp Development Tutorial for HarmonyOS Shopping Projects - Style Selector
Hello everyone! Today, we still bring you the sharing of the HarmonyOS cross-platform development tutorial. The final goal of this series of tutorials is to create a shopping application. Through this project, we will share with you the completion process of developing a HarmonyOS application through uniapp, from configuring the development environment to packaging and listing the application.

Yesterday's article implemented the carousel image on the application's home page, which involved setting the style for the carousel image. Here's a short piece of code to help you review it:
```
.swiper-item{
  width: 100%;
  height: 100%;
  display: block;
  text-align: center;
}
```
For students with a foundation in css, this code is quite simple. However, for beginners, it might not be very clear. Today, Youlan Jun will share with you the style selector when developing HarmonyOS with uniapp.

Style selectors are used to set styles for components, such as background color, size, spacing, etc. ArkTs also has these styles, and even their names are similar, but there are significant differences in syntax. Moreover, there are many types of selectors in uniapp. Let's explain them one by one below. Let's take setting the color of the text as an example:

#### Class selector

This first selector is the style in the above code, composed of ".+ name ". It modifies the component whose class is the corresponding name, for example:
```
<text class="text1">类选择器</text>

.text1{
  color: red;
}
```
In this way, the text in the corresponding component will be set to red.
#### ID selector

Slightly different from class selectors, ID selectors are composed of #+ names and modify the components corresponding to the ids:
```
<text id="text2">ID选择器</text>

#text2{
  color: green;
}
```
#### Attribute selector

Property selectors are rather straightforward. If you directly write the component name when defining the style, all the styles of that component will be modified:

```
<text>属性选择器</text>

text{
 color: gray;
}
```
#### Inline selector

I wonder if everyone has noticed that there is another way to write style directly in a component regarding styles. This way is called an inline selector:
```
<text style="color: orange;">内联选择器</text>

```

#### Descendant selector

This approach is based on the attribute selector. If you write two names when using the attribute selector, for example:
```
view text{
}
```

Such a style will take effect in the text component within the view container.



There are many more types of selectors, and we won't list them all here. The common selectors are basically the ones mentioned above.



Now let's talk about the issue of priority. You may often have seen inline selectors appear simultaneously with other selectors, such as:

```
<text class="text1" style="color: orange;">选择器优先级</text>

.text1{
  color: red;
}
```
Both selectors have set the color of the text. So which one has a higher priority? The answer is that the inline selector has a higher priority. Not only compared with class selectors, but among all the selectors we introduced above, inline selectors have a higher priority.

But it is not the one with the highest priority. What if the styles in my class selector and ID selector do not want to be overridden by inline selectors? It can be added after the style! important, like this:
```
text{
  color: gray !important;
}
```
Among all the selectors,! The priority of "important" is the highest, which also reminds everyone to use it with caution. Although it is easy to use, it has more disadvantages. If possible, it should not be used.

The above is some introduction to the selector. Thank you all for reading.
