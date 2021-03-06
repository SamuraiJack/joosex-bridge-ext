Name
====

JooseX.Bridge.Ext - metaclass, allowing you subclass ExtJS classes using Joose notation (or modify standard ExtJS classes) 


SYNOPSIS
========

        <!-- Joose  -->
        <script type="text/javascript" src="/jsan/Task/Joose/Core.js"></script>
        
    
        <!-- ExtJS bridged  -->
        <script type="text/javascript" src="ext-3.3.1/adapter/ext/ext-base.js"></script>
        
        <script type="text/javascript" src="/jsan/JooseX/Bridge/Ext.js"></script>
        <script type="text/javascript" src="/jsan/JooseX/Bridge/Ext/Convertor.js"></script>
        
        <script type="text/javascript" src="ext-3.3.1/ext-all.js"></script>
        
        <!-- eof ExtJS bridged  -->
        


        Class('Test.Run.Harness.Browser.UI.TestGrid', {
        
            xtype   : 'testgrid', 
            
            isa     : Ext.grid.GridPanel,
            
            does    : ExtX.Some.Role.For.Grid,
            
            ....
        })
        
        
        Ext.Container.meta.extend({
            does : ExtX.Some.Role.For.Container
        })
        

INSTALLATION
============

See [Joose docs](http://joose.github.com/Joose) for installation instructions. 

From `npm`:
    
    > [sudo] npm install joosex-bridge-ext --unsafe-perm=true
    
Or you can download the tarball from one the sites:

<http://nodul.es/modules/joosex-bridge-ext>

<http://npm.mape.me/> (filter the modules for `joosex-bridge-ext` term)



DESCRIPTION
===========

`JooseX.Bridge.Ext` is a custom metaclass, which bridges the class system of ExtJS framework to Joose (or vice-versa, as you prefer).

It transparently turns all ExtJS classes into Joose classes, and allows you to use any of Joose features for them.
Pleas refer to Joose [documentation](http://joose.github.com/Joose/), to know why you might want to do that :)

This extension will work with unmodified ExtJS sources! 


EXAMPLES
========

        Class('Test.Run.Harness.Browser.UI.TestGrid', {
        
            // custom xtype, if not provided - class name will be used
            xtype   : 'testgrid', 
            
            isa     : Ext.grid.GridPanel,
            
            does    : ExtX.Some.Role.For.Grid,
            
            trait   : JooseX.CPS,
            
            
            has : {
                harness   : {
                    is          : 'rw',
                    required    : true
                }
            },
            
            
            before : {
                initComponent : function () {
                    var sm = new Ext.grid.CheckboxSelectionModel({ singleSelect : false })
                    
                    this.addEvents('rowselect')
                    
                    Ext.apply(this, {
                        ....
                    })
                }
            },
            
            
            after : {
                initComponent : function () {
                    ....
                }
            },
            
            
            methods : {
                
                onRowSelect : function (selModel, indx, record) {
                    this.fireEvent('rowselect', this, record)
                }
            }
            
        })

USAGE
=====

First, include Joose on your page. Then, between ExtJS adapter and main sources, insert this package and one of the converters.

This package comes with two converters : `JooseX.Bridge.Ext.Convertor` and `JooseX.Bridge.Ext.LazyConvertor`.

The 2nd converter requires `JooseX.Meta.Lazy` installed, and makes all classes lazy. Please refer to the section below for details.


`xtype` builder
---------------

You can use new builder `xtype` in your class declaration. It will register your class with `Ext.reg` call.
If you will not specify `xtype`, your class will be registered using its name. 


Metaclass inheritance
---------------------

By default, when Joose class inherit from superclass, it will be created using the metaclass of the parent.
So generally, when subclassing ExtJS classes you can omit the metaclass (the 3rd example). Sometimes though,
the metaclass needs to be explicitly specified (see [Explicit metaclass specification])



CAVEATS
========

This metaclass unifies two class systems, which uses different approaches to class construction. Thus appears some caveats.
Make sure you've read this section if you have any problems using the bridge.


Explicit metaclass specification
--------------------------------

Some of the classes in ExtJS framework are defined outside of its class system, using "raw JavaScript".
Such classes will have the low-level metaclass `Joose.Proto.Class` or no meta-class at all. If you wish to inherit from such class, 
you will need to explicitly specify the metaclass, to use any advanced Joose feature. This should be a rare case for ExtJS > 3.0, but still.

An example will be inheritance from Ext.Template:

        Class('My.Template', {
            //explicit metaclass
            meta    : JooseX.Bridge.Ext,
            
            isa     : Ext.Template
        })


(OPTIONAL) COMBINING THIS EXTENSIONS WITH JooseX.Meta.Lazy 
==========================================================

Its known fact, that ExtJS class hierarchy is huge and somewhat bloated. Often you won't use many classes included in the `ext-all.js` package.
But all those classes will take some time for initialization, despite never used (at least immediately). 
Making them "lazy" will save you some startup time ("Lazy" classes are classes, which construction is delayed until the first instantiation). 

However ExtJS was written in assumption that all classes are "eager", so if you are going to use JooseX.Meta.Lazy you need to
use patched version of ExtJS. This version is available as the 'task-extjs' package from the [npm](http://npmjs.org) distribution platform.
At the moment patched version contains ExtJS 3.1.0 version. 

        <!-- Joose  -->
        <script type="text/javascript" src="/jsan/Task/Joose/Core.js"></script>
        
        
        <!-- JooseX.Meta.Lazy extension -->
        <script type="text/javascript" src="/jsan/JooseX/Meta/Lazy.js"></script>
    
        <!-- Ext bridge  -->
        
        <script type="text/javascript" src="/jsan/Task/ExtJS/Adapter/Ext.js"></script>
        
        
        <script type="text/javascript" src="/jsan/JooseX/Bridge/Ext.js"></script>
        <script type="text/javascript" src="/jsan/JooseX/Bridge/Ext/LazyConvertor.js"></script>
        
        <script type="text/javascript" src="/jsan/Task/ExtJS/All.js"></script>
        
        <!-- eof Ext bridge  -->
        


        Class('Test.Run.Harness.Browser.UI.TestGrid', {
        
            xtype   : 'testgrid', 
            
            isa     : Ext.grid.GridPanel,
            
            does    : ExtX.Some.Role.For.Grid,
            
            ....
        })
        
        
        Ext.Container.meta.extend({
            does : ExtX.Some.Role.For.Container
        })



GETTING HELP
============

This extension is supported via github issues tracker: <http://github.com/SamuraiJack/joosex-bridge-ext/issues>

You can also ask questions at IRC channel : [#joose](http://webchat.freenode.net/?randomnick=1&channels=joose&prompt=1)
 
Or the mailing list: <http://groups.google.com/group/joose-js>


SEE ALSO
========

Joose web-site: <http://joose.it>

General documentation for Joose: <http://joose.github.com/Joose/>

JooseX.Meta.Lazy documentation: <http://samuraijack.github.com/JooseX-Meta-Lazy>


BUGS
====

All complex software has bugs lurking in it, and this module is no exception.

Please report any bugs through the web interface at <http://github.com/SamuraiJack/joosex-bridge-ext/issues>



AUTHORS
=======

Nickolay Platonov [nplatonov@cpan.org](mailto:nplatonov@cpan.org)



COPYRIGHT AND LICENSE
=====================

Copyright (c) 2009-2010, Nickolay Platonov

All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
* Neither the name of Nickolay Platonov nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission. 

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE. 
