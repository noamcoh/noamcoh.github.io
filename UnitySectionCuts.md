# Section Cut for Unity

A crucial tool for designers is the ability to view a certain part of their product, this tool is called **Section Cut** and is widely implemented in various design programs such as Rhino, Solidworks, Twinmotion etc.

A detailed explaination of section cuts can be found in the following [link](https://themagnumgroup.net/Blog/section-drawing/).

The desired goal is the following cut:

![](https://themagnumgroup.net/Blog/wp-content/uploads/2020/04/cutting-plane.jpg) _The image is taken from the blog linked above_

As it can be seen in the image, the mesh is cut where it intersects with the plane, leaving two different parts.
The important part of section cutting is discarding one of those parts.

In Unity, and in graphical programming in general, the 3D mesh that we render is undergoing a process where it is divided into pixels, each pixel is assigned a color.
In our case, we will need to find a way to determine whether the pixel that we are examining right now is in the "correct" side of the plane, otherwise we should discard it.

The mathematical formula for that is quite simple and is explained in depth in the following [link](https://mathinsight.org/distance_point_plane).

Leaving us with the following two equations to solve:
D coeff equation: $ D = (-Xn _ X0) - (Yn _ Y0) - (Zn \* Z0)$
Distance equation: $(Ax1 + By1 + Cz1 + D)/Magnitude(N)$
Where Plane normal = (Xn, Yn, Zn), A point on the plane = (X0, Y0, Z0), and pixel in question = (X1, Y1, Z1).

For every pixel we have to calculate it's original location in the 3D world (X1, Y1, Z1) and then calculate the distance from the plane, if it is greater than 0, then it means it is in the "Correct" side of the plane and should be conserved, otherwise, it will be culled.

In Unity I have implemented this logic using a "Shader Graph"
![alt](/assets/projects/UnitySectionCuts/ShaderGraph.png)

As it can be seen in the graph, I'm calculating D at first and then I'm calculating the distance of the pixel from the plane by accessing the `Absolute World Space` node of the shader.
If the distance is greater than 0 then the alpha of the shader will be 1 which means completly opaque, otherwise a 0 will be used deeming the pixel as completely transparent, hence culled.

Lastly, I written a script that will update the shader of the plane's position and rotation in real time to allow adjustments of the section cut.

Try it yourself by cloning the [repo](https://github.com/noamcoh/SectionCuts).
