---
layout: post
title: Pie Coordinates
---

![Pie Coordinates](/assets/pie.png)

```objc
-(void)drawRect:(CGRect)rect {
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    
    CGPoint center = CGPointMake(rect.size.width / 2, rect.size.height / 2);
    CGFloat radius = rect.size.height/2;
    
    CGContextAddArc(ctx, rect.size.width - radius, center.y, radius, -M_PI_2, M_PI_2, NO);
    CGContextAddArc(ctx, radius, center.y, radius,M_PI_2, -M_PI_2, NO);
    CGContextSetFillColorWithColor(ctx, [UIColor disableColor].CGColor);
    CGContextFillPath(ctx);
}
```
