# Setting up Navigation Stack for Multiple Turtle Bots

1. Navigate to /turtlebot3_navigation/param/

2. You will need to add copies of the following files depending on the namespace of the turtlebot. 
    * global_costmap_params_tb3_#.yaml
    * local_costmap_params_tb3_#.yaml

    given that # the number of the turtlebot. Be sure to actually edit the **contents** of the files to match the name space as well.
