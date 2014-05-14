---
layout: post
title: IOS - Animating a Pie Chart using CALayer
---

![Pie Coordinates](/assets/pie.png)


Drawing a half circle using CGContextAddArc
---------------------

`XNCircleLayer.m:`

```objc
#import "XNCircleLayer.h"

@implementation XNCircleLayer

- (XNCircleLayer*) init {
    self = [super init];
    if (self) {
        //scale to correct for blurriness in retina
        self.contentsScale = [[UIScreen mainScreen] scale];
        //display circle
        [self setNeedsDisplay];
    }
    return self;
}

-(void)drawInContext:(CGContextRef)ctx {
    //get the circle bounds
    CGRect rect = self.bounds;
    //radius will be the size of half the layers bounds so entire
    //circle can fit inside the display bounds.
    CGFloat radius = CGRectGetMidX(rect);
	
	//first move to the center of the rect before drawing the arc
	    CGContextMoveToPoint(ctx, CGRectGetMidX(rect), CGRectGetMidY(rect));
    //draw the arc starting from the top (-M_PI_2) and 
	//end at bottom (M_PI_2)
    CGContextAddArc(ctx, CGRectGetMidX(rect), CGRectGetMidY(rect), radius, -M_PI_2, M_PI_2, NO);
    //fill the half circle we just created
    CGContextFillPath(ctx);
}

@end
```

`XNViewController.m:`

```objc
#import "XNViewController.h"
#import "XNCircleLayer.h"

@interface XNViewController ()
@property (strong,nonatomic) XNCircleLayer *circle;
@end

@implementation XNViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
    //Add our new Circle to the view
    self.circle = [XNCircleLayer new];
    self.circle.frame = CGRectMake(50, 50, 200, 200);
    [self.view.layer addSublayer:self.circle];
}

@end

```

`Output:`

![Pie Coordinates](/assets/half_pie.png)

Animating our circle
---------------------

`XNCircleLayer.h:`

```objc
#import <QuartzCore/QuartzCore.h>

@interface XNCircleLayer : CAShapeLayer
@property (assign, nonatomic) float endAngle;
@end
```

`XNCircleLayer.m:`

```objc
+ (BOOL)needsDisplayForKey:(NSString *)key {
    //make sure that the key matches the @dynamic endAngle name
    if ([key isEqualToString:@"endAngle"]) {
        return YES;
    }
    return [super needsDisplayForKey:key];
}
```

`XNCircleLayer.m:`

```objc
- (id)actionForKey:(NSString *)key {
    // make sure that the key matches the @dynamic endAngle name
    if ([key isEqualToString:@"endAngle"]) {
        // Create a basic interpolation for "endAngle" animation
        CABasicAnimation *animation = [CABasicAnimation
                                       animationWithKeyPath:key];
        // Animate the property
        animation.duration = 0.3f;
        animation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseOut];
        animation.fromValue = [[self presentationLayer] valueForKey:key];
        return animation;
    } else {
        return [super actionForKey:key];
    }
}
```

`XNCircleLayer.m:`

```objc
-(void)drawInContext:(CGContextRef)ctx {
    //get the circle bounds
    CGRect rect = self.bounds;
    //radius will be the size of half the layers bounds so entire
    //circle can fit inside the display bounds.
    CGFloat radius = CGRectGetMidX(rect);
    
    //first move to the center of the rect before drawing the arc
    CGContextMoveToPoint(ctx, CGRectGetMidX(rect), CGRectGetMidY(rect));
    //draw the arc starting from the top (-M_PI_2) and end at endAngle
    CGContextAddArc(ctx, CGRectGetMidX(rect), CGRectGetMidY(rect), radius, -M_PI_2, self.endAngle, NO);
    //fill the half circle we just created
    CGContextFillPath(ctx);
}
```

`XNViewController.h:`

```objc
- (void)viewDidLoad
{
    [super viewDidLoad];
    //Add our new Circle to the view
    self.circle = [XNCircleLayer new];
    self.circle.frame = CGRectMake(50, 50, 200, 200);
    [self.view.layer addSublayer:self.circle];
    
    self.circle.endAngle = M_PI_2;
    UIGestureRecognizer *g = [[UITapGestureRecognizer alloc]
                              initWithTarget:self
                              action:@selector(tap:)];
    [self.view addGestureRecognizer:g];
}

- (void)tap:(UIGestureRecognizer *)recognizer {
    self.circle.endAngle = M_PI;
}
```