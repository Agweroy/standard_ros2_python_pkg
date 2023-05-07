 # HOW TO CREATE A PYTHON PACKAGE IN ROS2  

In this repo you'll learn how to create and setup a ROS2 python package. 

I'll show you every procedure, and explain the relation between files, where  to write your nodes, how to add launch files, and so on.

***TABLE OF CONTENTS***

1. Setup your ROS2 Python package
2. Explanation of files inside the ROS2 Python package

    2.1. package.xml
    2.2. setup.py
    2.3. setup.cfg
    2.4. <package_name>/ folder
    2.5. resource/<package_name> file
    2.6. test/ folder

3. Compile your package
4. Build a Python node inside a ROS2 Python package
5. Install other files in a ROS2 Python package
      
    5.1. Launch files
    5.2. YAML config files

6. ROS2 Python package: going further

# Setup your ROS2 Python package

Before you can create a ROS2 Python package, 
  
   1. correctly installed ROS2,
   2. source your environment (```$ source /opt/ros/version_of_ros/setup.bash```) version_of_ros could be foxy or humble etc.
   3. then make sure you create a workspace
   $ mkdir -p ~/ros2_ws/src
   $ cd ros2_ws/
   $ colcon build

Now that the environment is setup, let's create a Python package:

In your ```~/ros2_ws/src/```, do:
        
    $ ros2 pkg create python_pkg

You should see this display on terminal:

going to create a new package
package name: python_pkg
destination directory: /home/agwe/ros2_ws/src
package format: 3
version: 0.0.0
description: TODO: Package description
maintainer: ['agwe <agweagwe45@gmail.com>']
licenses: ['TODO: License declaration']
build type: ament_python
dependencies: []
creating folder ./python_pkg
creating ./python_pkg/package.xml
creating source folder
creating folder ./python_pkg/python_pkg
creating ./python_pkg/setup.py
creating ./python_pkg/setup.cfg
creating folder ./python_pkg/resource
creating ./python_pkg/resource/python_pkg
creating ./python_pkg/python_pkg/__init__.py
creating folder ./python_pkg/test
creating ./python_pkg/test/test_copyright.py
creating ./python_pkg/test/test_flake8.py
creating ./python_pkg/test/test_pep257.py

Use ```ros2 pkg creat``` followed by the name of your package. Then add the option --buid-type ament-python to specify that you are building a ))Python package.

A system of files with four directories will be created in the new package as shown bellow.

.
└── python_pkg
    ├── package.xml
    ├── python_pkg
    │   └── __init__.py
    ├── resource
    │   └── python_pkg
    ├── setup.cfg
    ├── setup.py
    └── test
        ├── test_copyright.py
        ├── test_flake8.py
        └── test_pep257.py

# Explanation of files inside a ROS2 Python package

Here's a simple explanation for each file, and what you have to do in order to set them up.

# package.xml

this file provides some information about the develloper and the required dependencies for the package.


<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>python_pkg</name>
  <version>0.0.0</version>
  <description>TODO: Package description</description>
  <maintainer email="agweagwe45@gmail.com">agwe</maintainer>
  <license>TODO: License declaration</license>

  <test_depend>ament_copyright</test_depend>
  <test_depend>ament_flake8</test_depend>
  <test_depend>ament_pep257</test_depend>
  <test_depend>python3-pytest</test_depend>

  <export>
    <build_type>ament_python</build_type>
  </export>
</package>

You need to manually edit lines 5-8 Everything will work just fine if you don't do it, but if you decide to share or publish your package, then it is mandatory to do so.

1. version
2. description: a brief description of what your package does.
3. maintainer: name and email of current maintainer. You can add multiple 
maintainer tags. Also, you can add some author tags(with name and email) if you want to make the distinction between authors and maintainers.
4. license: if you ever want to publish your package you'll need a license (for example BSD, MIT, GPLv3).

# setup.py

If you know what a CMakeLists.txt file is, well the setup.py is basically the same but for Python. When you compile your package it will what to install, where to install, where to install it, how to link dependencies, etc.

from setuptools import setup

package_name = 'python_pkg'

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
    ],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='agwe',
    maintainer_email='agweagwe45@gmail.com',
    description='TODO: Package description',
    license='TODO: License declaration',
    tests_require=['pytest'],
    entry_points={
        'console_scripts': [
        ],
    },
)


We'll come back to this file later in this tutorial. For now you can see that the 4 lines we had to setup in the package.xml are also here. Modify those lines if you have to publish or share the package.

# setup.cfg

This file will tell where the scripts will be installed. Right now you have you have nothing to change.

  [develop]
  script-dir=$base/lib/python_pkg
  [install]
  install-scripts=$base/lib/python_pkg

# <package_name>/ foleder

This folder will be different every time, because it will always have the same name as your package. In this case the name of the package is "python_pkg", so the name of the folder is also "python_pkg".

You will create all your ROS2 Python nodes in this folder. Note that it already contains an empty __init__.py file.

# resource/python_pkg file.

This is needed for ROS2 to find your package. For our example the file name is "resource/python_pkg".

There is nothing to change in this file for now.


# test/ folder 

This folder, as its name suggests, is for testing. When you create a package it already contains 3 Python files.

** Compile your package.

To compile your package, go into your workspace directory and execute the command ```colcon build```.
Now tell ROS2 to only build our Python package with the command ```--package-select```.

    $ cd ~/ros2_ws
    $ colcon build --packages-select python_pkg

and you should see this display on your terminal.
   $ colcon build
     Starting >>> python_pkg
     Finished <<< python_pkg [4.28s]          
     Summary: 1 package finished [7.43s]

Note: When working with Python, you nay think that you don't need to compile anythong. That's true, you can directly execute the Python files that you create on visual studio code terminal without having to execute the command ```colcon build```. But compiling a package is much more than that: it will install the scripts in a place where they can find other modules from other packages, where they can be founc by other scripts. It will also allow you to start a node with ```row2 run```, add in a launch file, pass parameters to it, etc

Now that you know how to create and compile a package, let's make a few examples to see what you can do with this package.

# Build a Python node inside a ROS2 Python package.

Let's see how to build, and use a Python node, with our freshly created ROS2 Python package.

Create a file named py_node.py in the python_pkg/ folder.

   $cd ~/ros2_ws/src/python_pkg/python_pkg
   $ touch py_node.py

below is a Python script you can use for testing purpose

 import rclpy
 from rclpy.node

 class PythonNode(Node):
    def __init__(self):
        super.__init__("node_name")
        self.get_logger().info("Hello node")

    def main (args=None):
        rclpy.init(args=args)
        node = PythonNode()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == "__main__":
        main()

The node will just print a message when started, and then it will spin continously until you kill the node. If you want to know more about the code, check out how to write a python node in ROS2 here https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html

Now that we have a Python node, we have to add entry point in the setup.py file.

 entry_points={
        'console_scripts': [
                 
                 'test = python_pkg.py_node:main'
        
        ],

Find the "entry_points" dictionary and add one line in the "console_scripts" array.

  1. "test" will be the name of the executable after the script is installed 

  2. "python_pkg.py_node:main" means: execute the main() function inside the py_node.py file inside the python_pkg. So, the entry point is the main(). If you want to start your node. with a diffrent function, make sure to set the function name accordingly in setup.py
  3. Don't mix everything: executable name!=file name!=node name. Those are 3 different things. In our example: "test" is the executable. "py_node" is the file, and "node_name" is the name of the node. Note that you can also choose to use the same name for all three.
  4. The executable script will be installed in ~/ros2_ws/install/python_pkg/lib/python_pkg/. This is the folder specified in the setup.cfg file.

Once more thing you need to do: add a <depend>rclpy</depend> tag in package.xml, because we use a dependency to rclpy in our code. You only need to do this once per dependency for the whole package. If you create another node you'll need to update setup.py, but not package.xml if you don't have any new dependency.

And now you compile your package with ```colcon build --package-select python_pkg```. Then open new terminal, source your ROS2 workspace and execute the node with ```ros2 run```.

