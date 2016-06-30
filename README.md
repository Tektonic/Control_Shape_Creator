# Control_Shape_Creator
Control Shape Creator, Maya Python Scripting

#Control Shape Dev Tool
#
#Authors: Callum Macfarlane
#Contact: 1301948@abertay.ac.uk
#
#-----------------------------------------------
#
#Description: This tool is used to produce a variety of different control shapes for your desired joint.
#It will go through the procerss of renaming, creating offset groups and placing the control shape.
#You can change the shape and size of your control shape.
#To prevent any errors you may occur with not having your control shapes 0'ed out.
#
#To install this script create a shelf button using the following code:
#
#
#



#Delete UI if already and existing one
import maya.cmds
if maya.cmds.window(csc, exists=True):
    maya.cmds.deleteUI(csc)



#Create Window
csc = maya.cmds.window(title='Control_Shape_Creator')

#Column Layout
maya.cmds.columnLayout()

#Menu Bar
menuBarLayout = maya.cmds.menuBarLayout()

#Edit Menu
maya.cmds.menu( label='Edit' )
maya.cmds.menuItem( label='Save Settings' )
maya.cmds.menuItem( label='Reset Settings' )

#Help Menu
maya.cmds.menu (label='Help')
maya.cmds.menuItem( label='Help On Control Shape Creator' )


#Slider for Radius
my_value = maya.cmds.floatSliderGrp( field=True, label='Radius', minValue=0, maxValue=10, fieldMinValue=0, fieldMaxValue=10, value=0.5 ,cc=slider_drag_callback)


#Row Layout for bottom buttons
maya.cmds.rowLayout( numberOfColumns=3, columnWidth3=(80, 75, 150))
maya.cmds.button( label='Atom', w=150, command=controlshape)
maya.cmds.button( label='Circle',w=150, command=circleshape)
maya.cmds.button( label='Close', w=150, command=close)

#Display Window
maya.cmds.showWindow( csc )


maya.cmds.floatSliderGrp(my_value, query=True, value=True)
print my_value

#Command for Close Button
def close(self):
    
    if maya.cmds.window(csc, exists=True):
       maya.cmds.deleteUI(csc, window=True)



#Creating Atom Control Shape
#
#
#
#
#       
#
def controlshape(self):     

    mycircleR =  maya.cmds.floatSliderGrp(my_value, query=True, value=True)

#Call it 'selectedjnt'
    selectedjnt = maya.cmds.ls(selection=True)
    

#Create Circle using radius from slider
    maya.cmds.circle(center=(0, 0, 0), normal=(0, 1, 0), sweep=360, radius=mycircleR, degree=3, useTolerance=False, constructionHistory=True, n= 'crclmain'+selectedjnt[0])

#Delete History of circle
    maya.cmds.delete(constructionHistory=True)


#Duplicate Circles

    maya.cmds.duplicate( 'crclmain'+selectedjnt[0] , n= 'crclone'+selectedjnt[0])
    maya.cmds.duplicate( 'crclmain'+selectedjnt[0] , n= 'crcltwo'+selectedjnt[0])
    maya.cmds.duplicate( 'crclmain'+selectedjnt[0] , n= 'crclthree'+selectedjnt[0])
    maya.cmds.duplicate( 'crclmain'+selectedjnt[0] , n= 'crclfour'+selectedjnt[0])


#Rotating Circles to Position

    maya.cmds.select( 'crclone'+selectedjnt[0] )
    maya.cmds.rotate(0,0,45)

    maya.cmds.select( 'crcltwo'+selectedjnt[0] )
    maya.cmds.rotate(0,0,90)

    maya.cmds.select( 'crclthree'+selectedjnt[0] )
    maya.cmds.rotate(0,0,135)

    maya.cmds.select( 'crclfour'+selectedjnt[0] )
    maya.cmds.rotate(90,0,0)

    maya.cmds.select( 'crcl*'+selectedjnt[0] )

#Freezing Transformations    

    maya.cmds.makeIdentity( apply=True , t=1, r=1, s=1, n=2)


#Targetting shape node and main circle
#Parent them
#Delete the shape node
    maya.cmds.select( 'crclone'+selectedjnt[0]+'Shape' , 'crclmain'+selectedjnt[0] )
    maya.cmds.parent( r=True , s=True )
    maya.cmds.delete( 'crclone'+selectedjnt[0] )

    maya.cmds.select( 'crcltwo'+selectedjnt[0]+'Shape' , 'crclmain'+selectedjnt[0] )
    maya.cmds.parent( r=True , s=True )
    maya.cmds.delete( 'crcltwo' +selectedjnt[0])

    maya.cmds.select( 'crclthree'+selectedjnt[0]+'Shape' , 'crclmain'+selectedjnt[0] )
    maya.cmds.parent( r=True , s=True )
    maya.cmds.delete( 'crclthree' +selectedjnt[0])

    maya.cmds.select( 'crclfour'+selectedjnt[0]+'Shape' , 'crclmain'+selectedjnt[0])
    maya.cmds.parent( r=True , s=True )
    maya.cmds.delete( 'crclfour' +selectedjnt[0])

    maya.cmds.select( 'crclmain' +selectedjnt[0])


#Create Offset Group
#Call it 'offsetgrp'
    offsetgrp = maya.cmds.group(n = 'offset'+selectedjnt[0])



#Selecting Offset Group and Joint
    maya.cmds.select (offsetgrp,selectedjnt)

#Parent them
    maya.cmds.parent()
    
#selecting offset group
    selectedoffset = maya.cmds.ls(selection=True)
    
    print selectedoffset
    
    
#Set the translation and rotation to 0    
    maya.cmds.setAttr(".translateX", 0) 
    maya.cmds.setAttr(".translateY", 0) 
    maya.cmds.setAttr(".translateZ", 0) 
    maya.cmds.setAttr(".rotateX", 0) 
    maya.cmds.setAttr(".rotateY", 0) 
    maya.cmds.setAttr(".rotateZ", 0) 

#Unparent so the offset holds the non-zero values
    maya.cmds.Unparent()
   
    

#Creating Circle   
#
#
#
#
#
def circleshape(self):

#Call it 'selectedjnt'
    mycircleR =  maya.cmds.floatSliderGrp(my_value, query=True, value=True)

    selectedjnt = maya.cmds.ls(selection=True)

#Create circle using radius from slider
    maya.cmds.circle(center=(0, 0, 0), normal=(0, 1, 0), sweep=360, radius=mycircleR, degree=3, useTolerance=False, constructionHistory=True, n= 'crclmain'+selectedjnt[0])
    
    maya.cmds.delete(constructionHistory=True)
    
    maya.cmds.makeIdentity( apply=True , t=1, r=1, s=1, n=2)

#Select circle and joint
    maya.cmds.select( 'crclmain' + selectedjnt[0])
    
    
#Group them in offset grouo    
    offsetgrp = maya.cmds.group(n = 'offset'+selectedjnt[0])
    
    maya.cmds.select (offsetgrp,selectedjnt)
    
    
#Parent group and joint
    maya.cmds.parent()
    
    #selecting offset group
    selectedoffset = maya.cmds.ls(selection=True)
    
    
    
#Set the translation and rotation to 0    

    maya.cmds.setAttr(".translateX", 0) 
    maya.cmds.setAttr(".translateY", 0) 
    maya.cmds.setAttr(".translateZ", 0) 
    maya.cmds.setAttr(".rotateX", 0) 
    maya.cmds.setAttr(".rotateY", 0) 
    maya.cmds.setAttr(".rotateZ", 0) 
    
    
#Unparent so the offset holds the non-zero values

    maya.cmds.Unparent()
    
############


