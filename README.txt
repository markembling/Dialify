Dialify
=======

Overview
--------

Dialify is a jQuery plugin which replaces instances of the <meter> element with 
a <canvas>-drawn dial.

Features:
 - customisation of size and colours
 - customisation of min/max angle
 - customisation of which dial parts are drawn and their sizes
 - use an image to draw entirely custom dials


Usage
-----

To use the default settings, simply call the dialify method on the appropriate
meter elements:

	$('meter.mydial').dialify();
	
This creates a dial in a canvas of 100x100px, with a white dial drawn in it. The 
resulting canvas will be given a class of 'dialify-meter'.

Customisation options can be passed in like so:

	$('meter#mydial2').dialify({
		'width': 60,
		'height': 60,
		'class': 'my-awesome-dial'
	});

This will create a 60x60px canvas, which will be given a class of 'my-awesome-dial'.
The dial will use the defaults for any unspecified options.

The examples folder contains some practical demonstrations of various options.


Browser Support
---------------

Dialify should work in all browsers supporting the HTML5 canvas API. To make it 
work in IE 7/8, the ExplorerCanvas library will need to be used (http://code.google.com/p/explorercanvas/).
Unfortunately, there are some problems concerning rotation of images, so I have 
provided a version patched using http://dev.sencha.com/playpen/tm/excanvas-patch/.

This version should work correctly, and can be found in the lib folder. The examples 
make use of this to ensure they work in IE.


Options
-------

Options marked with an asterisk (*) have no meaning when a custom image is being 
used (see 'Using a Custom Image'), and are ignored.

Option						Default					
--------------------------- -----------------------
width						100
The width of the canvas - if the width & height do not match, the dial is drawn
in the centre, using the smaller of the two dimensions as a diameter.

height						100
The height of the canvas - if the width & height do not match, the dial is drawn
in the centre, using the smaller of the two dimensions as a diameter.

class						'dialify-meter'
The class given to the new canvas element

drawDialFace*				true
Draw the dace of the dial (outline, background and shaded scale area).

scaleArcRadius*				[automatically fits to the dial size]
This is the radius from the pointer rotation point to the outside edge of the 
shaded scale area. This allows the scale to be grown/shrunk independently of 
the overall dial size.

drawSpindle*				true
Draw the centre spindle (the part the dial is attached to).

dialFaceColor*				'#FFF'
The background colour for the dial face.

dialOutlineColor*			'#000'
The colour of the dial face outline.

scaleRangeColor*			'#DDD'
The colour of the shared scale area representing the range from min to max.

pointerColor*				'#000'
The colour of the dial's pointer.

spindleColor*				'#999'
The colour of the centre spindle to which the needle is attached.

spindleOutlineColor*		'#000'
The outline colour of the centre spindle

pointerWidth*				4
Thickness of the pointer.

pointerRotationPoint		[automatically determines the centre based on dial size]
Takes an object in the form { 'x': 50 , 'y': 50 }, representing the point around 
which the pointer rotates. Either component (x or y) may be omitted and the 
default will be used.
	
pointerRange				{ min: 0 - (Math.PI * 1.25), max: (Math.PI * 0.25) }
The angles in radians which make up the min and max points on the dial. These are 
specified in radians, and the zero point is at the 3 o'clock position. Like the 
pointerRotationPoint option, either part may be omitted. However it is best to 
specify them both. The default options place the min point between the 7 and 
8 o'clock position, and the max point between the 4 and 5 o'clock position.

pointerLength				[automatically determined based on the scaleArcRadius setting]
The length of the pointer, from the centre point to the tip.

image						[none]
This option specifies an image file to be used for rendering a custom dial image.
See the following section for full details.


Using a Custom Image
--------------------

Dialify offers the option of using an image for building up an entirely custom 
dial look-and-feel. This will normally be combined with the width/height options 
and the options for manipulating the pointer range and rotation point to build 
up the correct effect.

The image file contains all the parts needed for the dial including the background, 
the pointer and any overlay which is required (such as a spindle or a glossy cover).

The image dimensions should be as follows:

The letters/functions used below represent:
	x: the desired width of the dial
	y: the desired height of the dial
	Max(): returns the largest of the provided numbers

width: 
	2*x

height:
	1*y + Max(x, y)
	
The top left section of the image is where the background is drawn. This section 
is enclosed in the region defined by these points:
	top left: 0, 0
	bottom right: x, y
	
The top right section is where the overlay is drawn. This is the final part drawn 
on to the dial.
	top left: x, 0
	bottom right: x*2, y
	
Across the bottom of these two sections, a 1-pixel gap is left. This is intentional.

The remaining section of the image is where the pointer is drawn. This section 
is defined with the following corners:
	top left: 0, y+1
	bottom right: x*2, y + Max(x,y) + 1
	
The pointer should be drawn so that it is in the 3 o'clock position. The centre 
point is always assumed to be in the very centre of this section, at this point 
in the image:
	pointer rotation point: x, (y+1) + (Max(x, y) / 2)

See the examples folder for an example of this image. The pointer section is as 
large as it is to ensure it is possible to cater for any possibility, including 
that of having a pointer which has a counter-weight on the other side, or consists 
of a disc with a hole marking the current point. This should all be possible.
