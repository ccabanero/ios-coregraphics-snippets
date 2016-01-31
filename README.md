#ios-coregraphics-snippets

##Description

This repo simply contains sample code for using Core Graphics with Swift.

##Languages

Swift 2

####Jump to a Topic

[Drawing an arc](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-an-arc)

[Drawing an oval](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-an-oval)

[Drawing a line](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-a-line)

[Drawing a triangle](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-a-triangle-path)

[Drawing stars](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-stars)

[Drawing a rectangle](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-a-rectangle)

[Drawing a gradient](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-a-gradient)

[Clipping a gradient with a Path](https://github.com/ccabanero/ios-coregraphics-snippets#clipping-a-gradient-with-a-path)

####Drawing an arc

![icon](imgs/arc.png)

````
@IBDesignable
class MyView: UIView {

    override func drawRect(rect: CGRect) {

        //Draw the main path
        let color = UIColor.orangeColor()

        // center of view
        let center = CGPoint(x:bounds.width/2, y: bounds.height/2)

        // derived from max width/height dimensions of view
        let radius: CGFloat = max(bounds.width, bounds.height)

        // defines thickness of the arc
        let arcWidth: CGFloat = 50

        // start and end angles of arc
        let startAngle: CGFloat = 3 * π / 4 //note: use alt + p to get pi
        let endAngle: CGFloat = π / 4

        // create path based on center, radius, and angles
        let path = UIBezierPath(arcCenter: center,
        radius: radius/3 - arcWidth/3,
        startAngle: startAngle,
        endAngle: endAngle,
        clockwise: true)

        // set line width
        path.lineWidth = arcWidth
        color.setStroke()

        // stroke the path
        path.stroke()
    }
}
````

####Drawing an oval

![icon](imgs/oval.png)

````
@IBDesignable
class OvalView: UIView {

    @IBInspectable var fillColor: UIColor = UIColor.greenColor()

    override func drawRect(rect: CGRect) {
        
        // create oval path
        let ovalPath = UIBezierPath(ovalInRect: rect)

        // give the current context a fill color
        fillColor.setFill()

        // fill the oval path
        ovalPath.fill()
    }
}
````

####Drawing a line
![icon](imgs/line.png)

````
@IBDesignable
class CustomView: UIView {

    override func drawRect(rect: CGRect) {

        // get the graphics context for the view
        let context = UIGraphicsGetCurrentContext()

        // line width
        CGContextSetLineWidth(context, 4.0)

        // stroke color
        CGContextSetStrokeColorWithColor(context, UIColor.yellowColor().CGColor)

        // line start
        CGContextMoveToPoint(context, 20, 20)

        // line end
        CGContextAddLineToPoint(context, 600, 600)

        // draw line
        CGContextStrokePath(context)
    }
}
````

####Drawing a triangle (path)
![icon](imgs/triangle.png)

````
@IBDesignable
class CustomView: UIView {

    override func drawRect(rect: CGRect) {

        // get the graphics context for the view
        let context = UIGraphicsGetCurrentContext()

        // line width
        CGContextSetLineWidth(context, 4.0)

        // stroke color
        CGContextSetStrokeColorWithColor(context, UIColor.yellowColor().CGColor)

        // triangle path
        CGContextMoveToPoint(context, 150, 50)
        CGContextAddLineToPoint(context, 250, 250)
        CGContextAddLineToPoint(context, 50, 250)
        CGContextAddLineToPoint(context, 150, 50)

        // draw line
        CGContextStrokePath(context)
    }
}
````

####Drawing stars

![icon](imgs/stars.png)

````
@IBDesignable
class CustomView: UIView {

    override func drawRect(rect: CGRect) {

        drawStars(5)
    }

    private func drawStars(numberOfStars: Int) {

        let context = UIGraphicsGetCurrentContext();

        var xCenter = 40.0;
        let yCenter = 150.0;

        let w  = 50.0;
        let r = w / 2.0;
        let flip = -1.0;
        let PI = 3.14159265359

        for _ in 1...numberOfStars {

            CGContextSetFillColorWithColor(context, UIColor.yellowColor().CGColor)

            let theta = 2.0 * PI * (2.0 / 5.0); // 144 degrees

            CGContextMoveToPoint(context, CGFloat(xCenter), CGFloat(r * flip + yCenter));

            for index in 1...4 {
                let x = r * sin(Double(index) * theta);
                let y = r * cos(Double(index) * theta);
                CGContextAddLineToPoint(context, CGFloat(x + xCenter), CGFloat(y * flip + yCenter));
            }

            let spaceBetweenStars = w + 10
            xCenter += spaceBetweenStars;
        }

        CGContextClosePath(context);
        CGContextFillPath(context);
    }
}
````

####Drawing a rectangle

![icon](imgs/rectangle.png)

````
@IBDesignable
class CustomView: UIView {

    override func drawRect(rect: CGRect) {

        let context = UIGraphicsGetCurrentContext()

        CGContextSetLineWidth(context, 8)

        CGContextSetStrokeColorWithColor(context, UIColor.cyanColor().CGColor)

        let rectangle = CGRectMake(60, 100, 200, 80)

        CGContextAddRect(context, rectangle)

        CGContextStrokePath(context)
    }
}
````

####Drawing a gradient

![icon](imgs/gradient_square.png)

````
@IBDesignable
class MyView: UIView {

    @IBInspectable var startColor: UIColor = UIColor.redColor()
    @IBInspectable var endColor: UIColor = UIColor.greenColor()

    override func drawRect(rect: CGRect) {

        // get the current context
        let context = UIGraphicsGetCurrentContext()

        // create gradient with - colors, color space, and color stop locations
        let colors = [startColor.CGColor, endColor.CGColor]
        let colorSpace = CGColorSpaceCreateDeviceRGB()
        let colorLocations:[CGFloat] = [0.0, 1.0]
        let gradient = CGGradientCreateWithColors(colorSpace, colors, colorLocations)

        // draw the background gradient
        let startPoint = CGPoint.zero
        let endPoint = CGPoint(x: 0, y: self.bounds.height)
        CGContextDrawLinearGradient(context, gradient, startPoint, endPoint, CGGradientDrawingOptions.DrawsAfterEndLocation)
    }
}
````

####Clipping a gradient with a Path

![icon](imgs/gradient_clip.png)

````
@IBDesignable
class MyView: UIView {

    @IBInspectable var startColor: UIColor = UIColor.redColor()
    @IBInspectable var endColor: UIColor = UIColor.greenColor()
    
    override func drawRect(rect: CGRect) {

        // round the top left and right corners by clipping with path
        let path = UIBezierPath(roundedRect: rect, byRoundingCorners: [UIRectCorner.TopLeft,  UIRectCorner.TopRight], cornerRadii: CGSize(width: 20.0, height: 20.0))
        path.addClip()

        // get the current context
        let context = UIGraphicsGetCurrentContext()

        // create gradient with - colors, color space, and color stop locations
        let colors = [startColor.CGColor, endColor.CGColor]
        let colorSpace = CGColorSpaceCreateDeviceRGB()
        let colorLocations:[CGFloat] = [0.0, 1.0]
        let gradient = CGGradientCreateWithColors(colorSpace, colors, colorLocations)

        // draw the background gradient
        let startPoint = CGPoint.zero
        let endPoint = CGPoint(x: 0, y: self.bounds.height)
        CGContextDrawLinearGradient(context, gradient, startPoint, endPoint, CGGradientDrawingOptions.DrawsAfterEndLocation)
    }
}