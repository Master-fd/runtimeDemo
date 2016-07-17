# runtimeDemo
通过runtime消息转发机制，详解objc_msgForward消息转发机制

runtime 机制，每个消息都会转换从obj_msgSend的形式
# obj_msgSend执行过程   
1、查找缓存  
2、查找方法列表  
3、查找父类方法列表   
4、转发_objc_msgForward

# _objc_msgForward执行过程
1.调用resolveInstanceMethod:方法，允许用户在此时为该Class动态添加实现。如果有实现了，则调用并返回。如果仍没实现，继续下面的动作。  
2.调用forwardingTargetForSelector:方法，尝试找到一个能响应该消息的对象。如果获取到，则直接转发给它。如果返回了nil，继续下面的动作。   
3.调用methodSignatureForSelector:方法，尝试获得一个方法签名。如果获取不到，则直接调用doesNotRecognizeSelector抛出异常。   
4.调用forwardInvocation:方法，将地3步获取到的方法签名包装成Invocation传入，如何处理就在这里面了。   
上面这4个方法均是模板方法，开发者可以override，由runtime来调用。最常见的实现消息转发，就是重写方法3和4，吞掉一个消息或者代理给其他对象都是没问题的
