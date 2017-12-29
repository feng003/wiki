---
title: '3、设计模式之观察者模式(Observer)'
date: 2016-09-17 15:50
---

> php 实现 观察者模式

#####     观察者模式 又被称作发布-订阅模式或者消息机制，定义了一种依赖关系，解决了主体对象与观察者之间功能的耦合。观察者对象有一个消息容器和 三个方法：订阅消息，取消订阅消息、发送订阅消息。

**demo0**

    interface Subject{
        //添加观察者
        public function Attach($observer);
        //踢出观察者
        public function Detach($observer);
        //满足条件通知观察者
        public function Notify();
        //观察条件
        public function SubjectState($subject);
    }

    /**
    * 观察类具体实现
    * Class Boss
    */
    class Boss implements Subject
    {
        public $_action;
        private $_Observer;
        public function Attach($observer)
        {
            $this->_Observer[] = $observer;
            // TODO: Implement Attach() method.
        }

        public function Detach($observer)
        {
            $ObserverKey = array_search($observer,$this->_Observer);
            if($ObserverKey !== false)
            {
                unset($this->_Observer[$ObserverKey]);
            }
            // TODO: Implement Detach() method.
        }

        public function Notify()
        {
            foreach($this->_Observer as $value)
            {
                $value->Update();
            }
            // TODO: Implement Notify() method.
        }

        public function SubjectState($subject)
        {
            $this->_action = $subject;
            // TODO: Implement SubjectState() method.
        }
    }

    /**
    * 抽象观察者
    * Class Observer
    */
    abstract class Observer{
        protected $_User;
        protected $_Sub;
        public function __construct($user,$sub)
        {
            $this->_User = $user;
            $this->_Sub  = $sub;
        }
    }

    /**
    * 观察者
    * Class StockObserver
    */
    class StockObserver extends Observer{
        public function __construct($user, $sub)
        {
            parent::__construct($user, $sub);
        }

        public function Update()
        {
            echo $this->_Sub->_action . $this->_User;
        }
    }

    $lee = new Boss(); //被观察者

    $initUser = new StockObserver('lee',$lee); //初始化观察者

    $lee->Attach($initUser);
    $lee->Attach($initUser);
    $lee->Detach($initUser);
    //$lee->Detach($initUser);

    $lee->SubjectState('what it is');

    $lee->Notify();

> javascript 实现观察者模式

##### 观察者模式主要是用来解决类或对象之间的耦合，解耦两个相互依赖的对象，使其依赖于观察者的消息机制。

**demo1**

    var Observer = (function(){
           var __message = {};
           return {
               regist:function(type,fn){
                   if(typeof __message[type] === 'undefined'){
                       __message[type] = [fn];
                   }else{
                       __message[type].push(fn);
                   }
               },
               fire:function(type,args){
                   if(!__message[type]) {
                       return;
                       var events = {
                                   type: type,
                                   args: args || {}
                               },
                               i = 0,
                               len = __message[type].length;
                       for (; i < len; i++) {
                           __message[type][i].call(this,events);
                       }
                   }
               },
               remove:function(type,fn){
                   if(__message[type] instanceof Array){
                       var i = __message[type].length-1;
                       for(;i>=0;i--){
                           __message[type][i] === fn && __message[type].splice(i,1);
                       }
                   }
               }
           }
       })();

       Observer.regist('test',function(e){
            console.log(e.type, e.args.msg);
       });
        Observer.fire('test',{msg:"传递参数"});

**demo2**

        (function(){
            function addMsgItem(e){
                var text = e.args.text,
                        ul = $('#msg'),
                        li = document.createElement('li'),
                        span = document.createElement('span');
                li.innerHTML = text;
                span.onclick = function(){
                    ul.removeChild(li);
                    Observer.fire('removeCommentMessage',{
                        num:-1
                    })
                };
                li.appendChild(span);
                ul.appendChild(li);
            }
            Observer.regist('addCommentMessage',addMsgItem);
        })();

        (function(){
                function changeMsgNum(e){
                    var num = e.args.num;
                    $('#msg_num').innerHTML = parseInt($('#msg_num').innerHTML+num)
                }
                Observer.regist('addCommentMessage',changeMsgNum);
                Observer.regist('removeCommentMessage',changeMsgNum);
        })();
        (function(){
            var text = $('#content');
            $('#user_submit').onclick = function()
            {
                if(text.value ===''){
                    return;
                }else{
            //                alert('1');
                }
            };
            var texts = text.value;
            console.log(text.value);
            Observer.fire('addCommentMessage',{text:texts,num:1});
        })();


[php观察者](http://www.phpddt.com/php/observer.html)
