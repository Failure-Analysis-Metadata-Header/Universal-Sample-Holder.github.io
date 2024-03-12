# Universal Sample Holder

In semiconductor failure analysis (FA), multiple analysis steps using different measurement methods across a possibly large chain of tools are typically carried out to identify the root cause failure. In order to enable a higher degree of automation, the machines used for the different analysis steps must be able to communicate with each other through a standardized interface. Such a standardized interface is delivered by the FA Standardized File Header (Link to github page) which provides a standard schema for the storage of metadata associated with analysis images or other measurement data types. 

Directly linked to the Standardized File Header and the automation process of workflows, is the Universal Sample Holder. 
Currently, in the state of the art workflow at the FA, each measurement tool has its own sample holder. This means that every time a sample needs to be moved from one tool to another, it has to be removed and reattached. This process is time-consuming and increases the risk of damaging the sample. Furthermore, this setup complicates the task for the FA engineer to relocate a previously identified failure location if the location is in the nano-scale or not directly visible in the next tool of the workflow. These issues are addressed by the Universal Sample Holder, which serves as a standardized sample holder for each measurement device in a FA lab. It facilitates the transformation of the failure coordinates from one tool to another using its alignment marks:

(Image of Sample Holder with the Alignment Marks)

# Coordinate Transformation using the Alignment Marks

In this section, the calculus of the coordinate transformation between an exemplary tool A and tool B, using the Alignment Marks of the Universal Sample Holder, is explained.
In general every transformation in a cartesian coordinate system with orthogonal axes from, e.g. a point $p_{tool A}$ to $p_{tool B}$ can be represented as chained rotations and translations relative to the initial (global) or local coordinate frame of point $p_{tool A}$: 

$p_{tool B} = R(\theta)p_{tool A} + v$, 

where $R(θ)$ is the rotation matrix with the angle $θ$ around one of the three axes $x$, $y$ and $z$ and $v$ denotes a translational displacement.
The coordinate transformation of a point on the sample attached to the Universal Sample Holder can be simplified to a 2D-transformation. This is because the sample is aligned to the ring of the Universal Sample Holder, where the Alignment Marks are micro milled. Furthermore, the holder is fixed to the stage of tool A and tool B, such that the orientaion of it's ring is co-incident to the horizontal plane of the cartesian coordinate systems of both tools, e.g. the $XY$-plane. In this case the $z$-coordinate becomes idle and every displacement of $p_{tool A}$ to $p_{tool B}$ can be described with a single rotation and translation where $R(\theta)$ is now a 2-dimensional matrix $\in \mathbb{R}^{2 \times 2}$ and denotes now the rotation matrix around the z-axis with angle θ.
The vector $v$ is the translational displacement in the $XY$ - plane. 

To perfrom this transformation, one need to find the transformation matrix $R(\theta)$ and translation vector $v$. These can be derived with the help of the Alignment Marks of the Universal Sample Holder. In particular, we consider the transformation between the Alignment Marks from tool A to tool B: 

$$\begin{bmatrix} mark1_{tool A\_x} \\\ mark1_{tool A\_y} \end{bmatrix} = \begin{bmatrix}
    r_{11} & r_{12} \\\
    r_{21} & r_{22}
\end{bmatrix}
\begin{bmatrix}
    mark1_{tool B\_x} \\\
    mark1_{tool B \_y}
\end{bmatrix} +
\begin{bmatrix}
    v_{x} \\\
    v_{y}
\end{bmatrix}, $$

We see from the system of equations, to get the rotation matrix and translation vector, we need to find the six unknowns ($r_{11}$, $r_{12}$, $r_{21}$, $r_{22}$, $v_{x}$, $v_{y}$). A system of equation requires six equations to unambiguously solve after six unknowns. The usage of three alignment marks milled at the ring of the holder delivers a uniquely solvable system of equations with six equations and six unknowns: 
$$Eq1: mark1_{tool B\_x} = r_{11} mark1_{tool A\_x} + r_{12} mark1_{tool A\_y} + v_{x},$$
$$Eq2: mark1_{tool B\_y} = r_{21} mark1_{tool A\_x} + r_{22} mark1_{tool A\_y} + v_{y},$$
$$Eq3: mark2_{tool B\_x} = r_{11} mark2_{tool A\_x} + r_{12} mark2_{tool A\_y} + v_{x},$$
$$Eq4: mark2_{tool B\_y} = r_{21} mark2_{tool A\_x} + r_{22} mark2_{tool A\_y} + v_{y},$$
$$Eq5: mark3_{tool B\_x} = r_{11} mark3_{tool A\_x} + r_{12} mark3_{tool A\_y} + v_{x},$$
$$Eq6: mark3_{tool B\_y} = r_{21} mark1_{tool A\_x} + r_{22} mark3_{tool A\_y} + v_{y},$$

The solution of this system of equations delivers the six unknowns ($r_{11}$, $r_{12}$, $r_{21}$, $r_{22}$, $v_{x}$, $v_{y}$)
to build the rotation matrix and translation vector. With which every point on the sample, recorded with tool A, can be transformed in the coordinates of tool B. 

# Usage at the tools #

An appropriate interface at the measurement tool needs to be installed to fix the Universal Sample Holder to the tools stage. Because of the modular design of the holder, this can be done at the different interfaces of the holder (link to 3d files). To use the coordinate transformation, the Alignment Marks need to be scanned once before each measurement and the tools while also its center coordinates need to be saved. This procedure can be automated at the tools if the Alignment Marks are roughly at the same position in the tools stage fixture before each measurement. The center coordinates of the Alignment Marks are transfered in the apropriate section in the Standardized File Header between the tools, see this example of a File Header from an image at a scanning acoustic microscope: 
  
<div align="center">
  <img src="Failure-Analysis-Metadata-Header/fa-metadata-schema/documentation/images/UniversalSampleHolder-AlignmentMarks.png"/>
  <img src="Failure-Analysis-Metadata-Header/fa-metadata-schema/documentation/images/Alignment_Marks_SubSection.PNG" width = "200" height="400"/>
  <img src="Failure-Analysis-Metadata-Header/fa-metadata-schema/documentation/images/Stage_Coordinates.PNG" width = "250" height="400"/>  
</div>

<!-- ![plot](documentation/images/UniversalSampleHolder-AlignmentMarks.png) ![plot](documentation/images/Header_Example_AlignmentMarkSubSection.png) -->

The calculation of the six uknowns for the coordinate transformation can be performed on an external application or directly at the tool's software. 



