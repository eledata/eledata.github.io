---
layout: post
keywords: Learn Python
description: Learn Python Exercise
title: "Learn Python Exercise One"
categories: Python
tags: Python
group: archive
---



Learn the argv and data input/output

Exp 1:

Using the argv. It is a argument variable. 

    
    '''
    Created on Sep 17, 2014
    
    @author: MHuang1
    '''
    
    #!/usr/bin/env python # Means the code can be use in diff plats
    # -*- coding: utf-8 -*- 
    
    
    from sys import argv
    
    script, first, second,third = argv
    
    print "The script is called:", script
    print "Your first variable is:", first
    print "Your second variable is:", second
    print "Your third variable is:", third
    
    #Result
    # python ex13.py first 2nd 3rd
    # The script is called: learning_python_sys_argv.py
    # Your first variable is: first
    # Your second variable is: 2nd
    # Your third variable is: 3rd

    ##################################################
    from sys import argv
    
    def test_argv():
        
        args = argv
        if (len(args) == 1):
            print args[0]
        elif len(args) == 2:
            print args[0], args[1]
        else:
            print "Too much values!"
            
    if __name__ == '__main__':
        test_argv()
    
    
Exp 2:

Using the rawinput function to capture the user input info.raw_input() default is string or char. If you want to input

int. Shoud add int(raw_input(input)) outside.

    '''
    Created on Sep 17, 2014
    
    @author: MHuang1
    '''
    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    from sys import argv
    
    # argv is a argument variable. argv[0]: script name, argv[1]: the name we provide.
    script,user_name = argv
    prompt = '> ' #just a string need to ini.
    
    print "Hi %s, I'm the %s script." %(user_name,script)
    print "I'd like to ask you a few questions."
    print "Do you like me %s?"%user_name
    likes = raw_input(prompt)
    
    print "Where do you live %s?"%user_name
    lives = raw_input(prompt)
    
    print "What kind of computer do you have?"
    computer = raw_input(prompt)
    
    print """
    Alright, so you said %r about like me.
    You live in %r. Not sure where that is.
    And you have a %r computer. Nice.
    """%(likes, lives, computer)
    
    
    #result
    # C:>python learning_rawinput.py raw_function
    # Hi raw_function, I'm the learning_rawinput.py script.
    # I'd like to ask you a few questions.
    # Do you like me raw_function?
    # >yes
    # Where do you live raw_function?
    # >xiamen
    # What kind of computer do you have?
    # >dell
    # 
    # Alright, so you said 'yes' about like me.
    # You live in 'xiamen'. Not sure where that is.
    # And you have a 'dell' computer. Nice.

