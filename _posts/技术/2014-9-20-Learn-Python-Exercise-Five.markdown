---
layout: post
keywords: Learn Python
description: Learn Python Exercise
title: "Learn Python Exercise Five"
categories: Python
tags: Python
group: archive
icon: key
---
{% include codepiano/setup %}

Python functions

There are many python's function types. Such as if-else, if-elif-else, for in, while, 

lambda etc. 

learning_python_func.py

    
    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    __author__ = "Huang Moyue"
    __version__ = "0.01"
    __date__ = "09/19/2014"
    __copyright__ = "Copyright 2014 Huang Moyue"
    
    """
    if else structure
    """
    import statsout
    
    
    def my_abs(x):
        if x >= 0:
            return x
        else:
            return -x
        
    """
    while structure
    """
    def my_power(x,y):
        tmp = 1
        while y > 0:
            y = y - 1
            tmp = tmp * x
        return tmp
    
    def my_power_5(x, y = 5):
        tmp = 1
        while y > 0:
            y = y - 1
            tmp = tmp * x
        return tmp
    
    """
    For loop structure
    """
    def my_for_in():
        temp = 0
        for i in range(2,10):
            for j in range(1,i):
                temp = temp + i + j
        return temp
    
    
    """
    Internal function
    range(), len()
    """
    
    def my_range_len():
        a = ['Mary', 'had', 'a', 'little', 'lamb']
        for i in range(len(a)):
            print i, a[i]
    
    
    """
    function doc
    """
    def func_doc():
        """
        test the function doc!
        print test_func_doc.__doc__
        """
        pass
    
    """
    lambda
    example: lambda var: logc 
    """
    def lambda_func():
        print "Show how lamanda work:", (lambda x:x*3)(4)
    
    '''
    fib function
    '''
    def fib(n):
        a, b = 0, 1
        while a < n:
            print a
            a, b = b, a+b
    
    def fib_list(n):
        a, b = 0, 1
        result = []
        while a < n:
            result.append(a)
            a, b = b, a+b
        '''
        return a list object
        '''
        return result 
    
    '''
    input argument contain list and xingcan
    '''
    def f(a, L=[]):
        L.append(a)
        return L
    
    
    #*argument means list
    #**keywords means dict
    def cheeseshop(kind, *arguments, **keywords):
        print "-- Do you have any", kind, "?"
        print "-- I'm sorry, we're all out of", kind
        #list, for loop to catch the data
        for arg in arguments:
            print arg
        print "-" * 40
        
        #dict, for loop to catch the data
        keys = sorted(keywords.keys())
        for kw in keys:
            print kw, ":", keywords[kw]
    
    #getattri
    def test_getattri(self):
        li = ['test1','test2']
        getattr(li, "append")("test3")
        print li
    
    def output(data, format="text"):
        output_function = getattr(statsout, "output_%s" % format, statsout.statsout_text)
        return output_function(data)

testing_func.py

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    __author__ = "Huang Moyue"
    __version__ = "0.01"
    __date__ = "09/19/2014"
    __copyright__ = "Copyright 2014 Huang Moyue"
    
    import unittest
    from unittest import TestCase
    
    from learnpy.learning_func import my_abs
    import learnpy.learning_func
    from learnpy.learning_func import lambda_func
    from learnpy.learning_func import cheeseshop
    
    class Test_learn_func(TestCase):
        
        def test_abs(self):
            self.assertEqual(5,my_abs(-5))
            
        def test_power(self):
            self.assertEqual(100, learnpy.learning_func.my_power(10,2))
            self.assertEqual(32,  learnpy.learning_func.my_power_5(2))
        
        def test_for_in(self):
            self.assertEqual(360,  learnpy.learning_func.my_for_in())
            
        def test_range_len(self):
            learnpy.learning_func.my_range_len()
        
        def test_func_doc(self):
            print learnpy.learning_func.func_doc().__doc__
            
        def test_lambda(self):
            lambda_func()
            
        def test_cheeseshop(self):
            cheeseshop('Limburger', 'It\' very runny, sir',
                       "It's really very, VERY runny, sir.",            
                       shopkeeper='Michael Palin',
                       client="John Cleese",
                       sketch="Cheese Shop Sketch")
            
            
    if __name__ == '__main__':
        unittest.main()
