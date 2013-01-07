---
layout: post
title: Styling Yii's CLinkPager Your Way
comments: true
tags:
- yii
- programming
- php
---
CLinkPager is a Yii class that handles pagination for you in a simple fashion.  However, it also comes with it's own predefined CSS.  If you want to style it with your own custom CSS, and get rid of that annoying "Go to Page:" prefix that is appended to the page numbers, then read on.

The CSS file that is used to style CLinkPager is specified in the class property $cssFile.  To not use a seperate CSS file and simply style it in your site's main CSS, set this property to false.  Next, if you want to get rid of that "Go to Page:" prefix that pops up every time a link pager is on one of your pages, set the class poperty $header = "".  Of course, you could put whatever you want in those quotes, I elected to not have any text.

To bring all this together, all you need to do is extend the CLinkPager class, set these properties, and then put your new class file in the components folder.  Then, everytime you need to use your new link pager, you specify the class as something else (tradition dictates it be LinkPager) instead of CLinkPager, and your new class will be used.

{% highlight php %}
<?php
class LinkPager extends CLinkPager
{
        public $cssFile = false;
        public $header = "";
}
{% endhighlight %}

Then to use your new class in a CListView widget, you can specify exactly what class you need like this:
{% highlight php %}
<?php
$this->widget('zii.widgets.CListView', array(
        'dataProvider'=>$dataProvider,
        'itemView'=>'_view',
        'template'=>"{items}\n{pager}",
        'pager'=>array('class'=>'LinkPager'),
));
{% endhighlight %}
