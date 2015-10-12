---
layout: post
keywords: Learn Python
description: Learn Python Exercise
title: "Learn Python Exercise Seven"
categories: Python
tags: Python
group: archive
icon: key
---
{% include codepiano/setup %}

Classes

From python doc

**Python’s class mechanism adds classes with a minimum of new syntax and semantics. It is a mixture of the class mechanisms found in C++ and Modula-3. Python classes provide all the standard features of Object Oriented Programming: the class inheritance mechanism allows multiple base classes, a derived class can override any methods of its base class or classes, and a method can call the method of a base class with the same name. Objects can contain arbitrary amounts and kinds of data. As is true for modules, classes partake of the dynamic nature of Python: they are created at runtime, and can be modified further after creation.**

**Learning_class.py**

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    __author__ = "Huang Moyue"
    __version__ = "0.01"
    __date__ = "09/20/2014"
    __copyright__ = "Copyright 2014 Huang Moyue"
    
    """
    class structure 
    class class_name:
        <statement - 1>
        <statement - 1>
    """
    
    class My_Class:
        #Called when the instance is created. 
        def __init__(self):
            self.data = []
            
    class Complex:
        def __init__(self,realpart,implpart):
            self.r = realpart
            self.i = implpart
            
    class Dog:
        def __init__(self, name):
            self.name = name
            self.tricks = []
            
        def add_trick(self, trick):
            self.tricks.append(trick)
            
    #Inheritance
    """
    inheritance class structure
    class DerivedClass(BaseClass):
    """
    
    class Animal(object):
        def run(self):
            print "Animal is running..."
        def __len__(self):
            return 1000
            
            
    class Cat(Animal):
        def run(self):
            print "Cat is running...."
        
        def eat(self):
            print "Cat eat rat...."
    
    class Horse(Animal):
        def run(self):
            print "Horse is running...."
        def eat(self):
            print "Horse is eat glass...."
    
    #:polymorphic testing. Put the father type in testing function as var.
    def run_twice(animal):
        animal.run()
        animal.run()

**testing_class.py**
    
    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    __author__ = "Huang Moyue"
    __version__ = "0.01"
    __date__ = "09/19/2014"
    __copyright__ = "Copyright 2014 Huang Moyue"
    
    
    import unittest
    from unittest import TestCase
    
    from learnpy.learning_class import My_Class
    from learnpy.learning_class import Complex
    from learnpy.learning_class import Dog
    from learnpy.learning_class import Animal
    from learnpy.learning_class import Cat
    from learnpy.learning_class import Horse
    from learnpy.learning_class import run_twice
    
    class Test_Class(TestCase):
        
        def test_init(self):
            x = My_Class()
            x = [1,2,3,4,5,6]
            print x
            
            y = Complex(5,6)
            print y.r, y.i
            
            
        def test_dog(self):
            cla = Dog("cla")
            cla.add_trick("trick_1")
            cla.add_trick("trick_2")
            
            print cla.name
            print cla.tricks
            
        
        def test_inheritance(self):
            ins_cat = Cat();
            ins_horse = Horse();
            
            ins_cat.run()
            ins_horse.run()
            
            ins_cat.eat()
            ins_horse.eat()
            
            
        def test_instance(self):
            test_a = list()
            test_b = Cat()
            test_c = Horse()
            test_d = Animal()
            
            if isinstance(test_a, list):
                print "test_a is the list type!"
            else:
                print "test_a is not the list type!"
            
            if isinstance(test_b, Cat):
                print "test_is the Cat type!"
            else:
                print "test_b is not the Cat type!"
                
            if isinstance(test_c, Horse):
                print "test_c is the Horse type!"
            else:
                print "test_c is not the Horse type!"
            
            if isinstance(test_d, Animal):
                print "test_d the Animal type!"
            else:
                print "test_d is not the Animal type!"
            
            if isinstance(test_d, Horse):
                print "test_d is the Horse"
            else:
                print "test_d is not the Horse type!"
            
            #Testing the polymorphic
            run_twice(test_b)
            run_twice(test_c)
            run_twice(test_d)
            
            #Like __**__() function just have sepcial use in the python
            #We can mock __len__() function
            #Let the sys know our class have length
            print  test_d.__len__()
            print  test_c.__len__()
            print  test_b.__len__()
            
            #dir()
            #This function will show the all methods and attributes 
            print dir(test_d)   
            
    if __name__ == '__main__':
        unittest.main()
    




        