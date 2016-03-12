---
layout: post
title: 打造Google Nexus 网站式的菜单
categories:
- Life
tags:
- javascript
- css
---

> 这篇文章主要是来自[queness](http://www.queness.com/)网站的[Recreate Google Nexus Menu](http://www.queness.com/post/14666/recreate-google-nexus-menu/)。具体内容有删减，添加了一些自己的理解。[demo](http://www.queness.com/post/14666/recreate-google-nexus-menu)、[源代码](http://www.queness.com/resources/archives/google-nexus-7-menu.zip)。

最近在Codrops网站读到了一篇关于制作Google Nexus 网站式的菜单的教程。这篇文章非常不错，原教程是用原生javascript来写的，我决定挑战下自己用jQuery来重新实现这个效果。

具体效果，可以先去google nexus官方网站看看，地址[Google Nexus Menu](http://www.google.com/nexus/)。从这个效果我们可以提炼总结这个效果的具体交互逻辑如下：

- 当鼠标滑过导航按钮的时候，只会展开导航按钮的部分。当点击子菜单的按钮的时候，才会完全展开菜单。
- 如果只是展开子菜单按钮的部分，那么当鼠标划出的时候，子菜单就会隐藏；而当子菜单是完全展开的时候，则只有点击子菜单以外的地方，才会隐藏。
- 支持移动设备的触摸事件。

把交互逻辑理清楚了，后面编码实现就容易多了。这里并没有100%模仿Google Nexus的样式风格，不过核心交互是一样的。

还有一个要注意的是，这里用的了CSS3，所以对于不支持CSS3的浏览器，就无能为力了。当然用纯javascript是可以实现的，但是还是要用最新的CSS3技术来实现它，挑战下自己。运行结果如下图所示：

![](http://pic.yupoo.com/reicky_v/DisoGK8C/medium.jpg)

首先是HTML部分。

###HTML###

正如下图你所看到的的，我们这个主要的结构是两部分，用两个*UL*列表来作为两个菜单的结构。如下：

![](http://pic.yupoo.com/reicky_v/DispFn6p/medium.jpg)

main nav结构

    <nav>
    <ul>
        <li><a href="#" class="icon icon-menu" id="btn-menu">Menu</a></li>
    </ul>
	</nav>

side nav结构

    <div id="sideNav">
    <ul>
        <li class="searchForm">
            <a href="#" class="icon icon-search">
                <span>
                    <input type="text" placeholder="Search" class="search" />
                </span>
            </a>
        </li>
        <li><a href="#" class="icon icon-home"><span>Parent</span></a>
            <ul>
                <li><a href="#"><span>Children</span></a></li>
            </ul>
        </li>
    </ul>    
	</div>    

###CSS###

这里主要是用到了CSS3中的transition，box-sizing和媒体查询这几个特性。动画部分主要是用transition来做的，我觉的CSS动画比javascript实现动画效果更要平滑一些。

我们把需要有动画效果的加上transition属性。

    #sideNav,
	#sideNav.showHalfMenu,
	#sideNav.showFullMenu,
	#sideNav ul ul li,
	#sideNav.showFullMenu ul ul li     {
	    -webkit-transition: 0.2s ease;
	    -moz-transition: 0.2s ease;            
	    -ms-transition: 0.2s ease;                        
	    transition: 0.2s ease;        
	}    

下面是其于的样式，这里就不详说了：

    html.cursor {
    cursor: pointer;
	}
	nav {
	    font-family: 'Roboto', sans-serif;
	    width: 100%;
	    height: 59px;
	    border-bottom:1px solid #ddd;
	    position: fixed;
	    top:0;
	    left:0;
	    z-index:20;
	    background-color:#ffffff;
	}
	    nav ul,
	    #sideNav ul,
	    #sideNav ul ul    {
	        margin:0;
	        padding:0;
	        list-style:none;
	    }
    
        nav li {
            margin:0;
            float:left;
            border-right:1px solid #ddd;
            font-size:18px;
        }
        
        nav a,
        #sideNav a {
            color:#5b6064;
            text-decoration:none;
            display:block;
            padding:10px 30px;
            height:59px;
            -webkit-box-sizing: border-box;
            -moz-box-sizing: border-box;
            -o-box-sizing: border-box;
            line-height:35px;
        }
        
        nav a:hover,
        #sideNav a:hover {
            color:#ffffff;
            background-color: #5b6064;
        }    
        
    #sideNav {
        position: fixed;
        left:-60px;
        top:59px;
        width: 60px;
        height:100%;
        border-right:1px solid #ddd;                        
        background-color:#ffffff;
        overflow-y: auto;    
    }
        
        #sideNav.showHalfMenu {
            left:0;            
        }
        
        #sideNav.showFullMenu {
            left:0;
            width: 311px;        
        }
            #sideNav.showFullMenu ul ul li {
                height:59px;                
            }    
    
    
        #sideNav > ul {
            width: 100%;        
            padding-bottom:60px;    
        }
    
        #sideNav ul li {
            width: 100%;
            margin:0;        
            font-weight:300;
        }
        
        #sideNav ul li a {
            border-bottom:1px solid #ddd;        
            padding-left:70px;
        }
        
        #sideNav ul li span {
            position: relative;
            top:3px;
        }        
        
        #sideNav ul ul li {
            overflow:hidden;
            height: 0;                
        }

###SEARCH###

这里要注意的一点的，子菜单的第一项是搜索栏，为了使搜索框的样式跟其它的子菜单栏目样式相匹配，和谐。我们对搜索框的占位文字样式(placeholder)同意进行了设置，如下：

     #sideNav input.search {
            font-family: 'Roboto', sans-serif;            
            border:0;
            outline:0;
            font-weight:300;
            background:transparent;
            color:#5b6064;            
        }
                
        input.search::-webkit-input-placeholder {
            color:#5b6064;        
        }
        input.search:-moz-placeholder {
            color:#5b6064;        
        }
        input.search::-moz-placeholder {
            color:#5b6064;        
        }
        input.search:-ms-input-placeholder {
            color:#5b6064;
        }
        
        #sideNav li.searchForm:hover input.search:focus,
        #sideNav li.searchForm:hover input.search::-webkit-input-placeholder {
            color:#fff;        
        }  

###RESPONSIVE###     

为了使导航在移动设备也能正常的工作，我们用CSS3的媒体查询进行了样式定制：

![](http://pic.yupoo.com/reicky_v/DisvEz7u/medium.jpg)

    @media only screen and (min-width: 200px) and (max-width: 480px) {
    nav a,
    #sideNav a {
        padding:10px 15px;
    }    
    
    nav a#btn-menu {
        padding:10px 30px;    
    }
    #sideNav.showFullMenu,
    #sideNav.showFullMenu li,
    #sideNav.showFullMenu    a     {
        width: 100%;
    }    
	}

###Javascript###

由于我们的动画效果是用CSS3实现的，所以javascript只是用来控制相关动画类的添加删除。这里用到的编程是函数式的编程方法，所以我们这里创建了下面这几个函数*showHalfMenu()*, *showFullMenu()*, *hideMenu()* 和 *toggleMenu*。

我们还用到了[mobile detection script](http://stackoverflow.com/a/11381730)这段脚本来检测设备是否是移动设备，从而决定是否支持触摸事件。

下面是具体的javascript脚本，相关代码作用可以看对应的注释，如下：

    $(function () {
    
    var GNmenu = {
        isMenuOpened: false,
        init : function () {
            var menuBtn = $('#btn-menu');
            
            // 不同设备调用不同的事件，主要是点击和触摸
            menuBtn.on('touchstart click', function (e) {
                e.stopPropagation();
                e.preventDefault();
                
                /* 如果子菜单是显示一部分的状态，那就显示完整菜单，否则就隐藏菜单 */
                if ($('#sideNav').hasClass('showHalfMenu') && !$('#sideNav').hasClass('showFullMenu')) {
                    GNmenu.showFullMenu();
                } else {
                    GNmenu.toggleMenu();                                                
                }
                
            });
            // 如果是桌面PC设备，则执行下面的代码
            if (!GNmenu.isMobile()) {
                menuBtn.bind('mouseover', function () {
                        GNmenu.showHalfMenu();
                });
                
                menuBtn.bind('mouseout', function () {
                        GNmenu.hideMenu();
                });    
            
                $('#sideNav').bind('mouseover', function () {
                        GNmenu.showFullMenu();
                });                                            
                // 点击菜单意外的部分，隐藏菜单
                GNmenu.bodyClick();
            }
                        
            // 搜索表单
            // 点击搜索框的时候，接触点击事件
            $('.searchForm input[type=text]').focus(function () {
                $('html').unbind('click');    
            }).blur(function() {
                GNmenu.bodyClick();                                                
            });
                                                    
        },
        
        bodyClick: function () {
        
            $('html').bind('click',function () {
                if (GNmenu.isMenuOpened) {
                    GNmenu.hideMenu();                                                        
                }
            });                    
        
        },
        
        toggleMenu: function () {
            if (!GNmenu.isMenuOpened) {
                GNmenu.showFullMenu();
            } else {
                GNmenu.hideMenu();                    
            }
        },
        
        showHalfMenu: function () {
            $('#sideNav').addClass('showHalfMenu');
            GNmenu.isMenuOpened = true;                    
        },
        
        showFullMenu: function () {
            $('#btn-menu').addClass('icon-menu-active');
            $('#sideNav').addClass('showFullMenu');
            $('html').addClass('cursor');                
            GNmenu.isMenuOpened = true;                            
        },
        
        hideMenu: function () {
            $('#btn-menu').removeClass('icon-menu-active');
            $('#sideNav').removeClass('showFullMenu showHalfMenu');        
            $('html').removeClass('cursor');                        
            GNmenu.isMenuOpened = false;                    
        },
        
        // 移动设备检测: http://stackoverflow.com/a/11381730                
        isMobile: function() {
            var check = false;
            (function(a){if(/(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows (ce|phone)|xda|xiino/i.test(a)||/1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i.test(a.substr(0,4)))check = true})(navigator.userAgent||navigator.vendor||window.opera);
            
            return check;                     
        }
        
    }
    
    // 
    GNmenu.init();
    
});    

其实实现它并不难，主要是把它的交互逻辑理解清楚，当然还是要勤加练习才能真正的掌握它的原理。你可以去Codrops网站看看这篇教程，它完全是用原生javascript来实现的。[地址](http://tympanus.net/codrops/2013/07/30/google-nexus-website-menu/)。

