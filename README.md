
# Safety Remote Control ROS Driver

[Safe Remote Control System](http://humanisticrobotics.com/products/safe-remote-control) by
[Humanistic Robotics](http://humanisticrobotics.com)

Usage
-----

Start HRI controller node.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
roslaunch hri_safety_sense hri_vsc.launch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Reading Controller Input**

Subscribe to `/joy` topic. `/joy` will return `sensor_msgs/Joy` message.

**Controller Feedback**

View service arguments:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
rossrv show `rosservice type /hri_src/key_value`
rossrv show `rosservice type /hri_src/key_string`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update text on display:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
# put controller into display mode
rosservice call /hri_src/key_value "Key:99 Value:1"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* `KEY_DISPLAY_MODE=99`
* `VALUE_DISPLAY_MODE_CUSTOM_TEXT=1`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.sh}
# Write text to first row in display
rosservice call /hri_src/key_string "Key:90 Value:'Some text'"
# Write text to second row in display
rosservice call /hri_src/key_string "Key:91 Value:'Some more text'"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* `DISPLAY_ROW_1=90`
* `DISPLAY_ROW_2=91`
