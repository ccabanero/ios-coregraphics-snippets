ios-coregraphics-snippets
======================

##Description

This repo simply contains sample code for using Core Graphics.

##Languages

Swift 2

####Jump to a Topic

[Drawing a gradient](https://github.com/ccabanero/ios-coregraphics-snippets#drawing-a-gradient)

####Drawing a gradient

````
@IBDesignable
class GraphView: UIView {

    @IBInspectable var startColor: UIColor = UIColor.redColor()
    @IBInspectable var endColor: UIColor = UIColor.greenColor()

    override func drawRect(rect: CGRect) {

        //set up background clipping area for rounded corners
        let path = UIBezierPath(roundedRect: rect, byRoundingCorners: UIRectCorner.AllCorners, cornerRadii: CGSize(width: 8.0, height: 8.0))
        path.addClip()

        // get the current context
        let context = UIGraphicsGetCurrentContext()

        // create gradient with - colors, colorSpace, color stop locations
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
