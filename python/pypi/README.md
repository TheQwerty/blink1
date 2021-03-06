Python3 Blink(1) library
========================

About this library
------------------

This is a rewrite of ThingM's original Python library. It includes the following modifications:

* 100% test coverage on all library components
* Python 3.x compatible
* Automatic installation via Python Package Index.
* Higher level control over the blink(1).
* Single implementation with pyusb, intended to be installed with admin access or virtualenv.

Installation
------------

Use the pip utility to fetch the latest release of this package and any additional components required in a single step:

    pip install blink1
    
Developer installation
----------------------

Having checked out the blink(1) library, cd to python/pypi and run the setup script:

    python setup.py develop
    
Use
---

The simplest way to use this library is via a context manager.

    import time
    from blink1.blink1 import blink1
    
    with blink1() as b1:
        b1.fade_to_color(100, 'navy')
        time.sleep(10)
        
When the blink1() block exits the light is automatically switched off. It is also possible to access the exact same
set of functions without the context manager:

from blink1.blink1 import Blink1
    import time
    
    b1 = Blink1()
    b1.fade_to_rgb(1000, 64, 64, 64)
    time.sleep(3)
    b1.fade_to_rgb(1000, 255, 255, 255)

Unlike the context manager, this demo will leave the blink(1) at the end of execution.

Gamma correction
----------------

The context manager supports a ''gamma'' argument which allows you to supply a per-channel gamma correction value.

    from blink1.blink1 import blink1
    
    with blink1(gamma=(2, 2, 2)) as b1:
        b1.fade_to_color(100, 'pink')
        time.sleep(10)
        
This example provides a gamma correction of 2 to each of the three colour channels. 
        
Higher values of gamma make the blink(1) appear more colorful but decrease the brightness of colours. 

White point correction
----------------------

The human eye's perception of color can be influenced by ambient lighting. In some circumstances it may be desirable
to apply a small colour correction in order to make colors appear more accurate. For example, if we were operating
the blink(1) in a room lit predimenantly by candle-light:

    with blink1(white_point='candle', switch_off) as b1:
        b1.fade_to_color(100, 'white')

Viewed in daylight this would make the Blink(1) appear yellowish, hoever in a candle-lit room this would be perceived
as a more natural white. If we did not apply this kind of color correction the Blink(1) would appear blueish.

The following values are acceptable white-points:

* Any triple of (r,g,b). Each 0 <= luminance <= 255
* Any color_temperature expressed as an integer or float in Kelvin
* A color temperature name.

The library supports the following temperature names:

* candle
* sunrise
* incandescent
* tungsten
* halogen
* sunlight
* overcast
* shade
* blue-sky