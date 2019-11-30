# Swing notes

# JFrame
JFrame is a class of `javax.swing` package extended by `java.awt.frame`, it adds support for JFC/SWING component architecture. It is the top level window, with border and a title bar. JFrame class has many methods which can be used to customize it.

JFrame class has many constructors used to create a JFrame. Following is the description.

`JFrame()`: creates a frame which is invisible

`JFrame(GraphicsConfiguration gc)`: creates a frame with a blank title and graphics configuration of screen device.

`JFrame(String title)`: creates a JFrame with a title.

`JFrame(String title, GraphicsConfiguration gc)`: creates a JFrame with specific Graphics configuration and specified title.

# Glossary

## Java Foundation Classes (JFC)

JFC encompass a group of features for building graphical user interfaces (GUIs) and adding rich graphics functionality and interactivity to Java applications. It is defined as containing the features shown in the table below.

|Feature |	Description|
|--------|-------------|
|Swing GUI Components |	Includes everything from buttons to split panes to tables. Many components are capable of sorting, printing, and drag and drop, to name a few of the supported features.|
|Pluggable Look-and-Feel Support | The look and feel of Swing applications is pluggable, allowing a choice of look and feel. For example, the same program can use either the Java or the Windows look and feel. Additionally, the Java platform supports the GTK+ look and feel, which makes hundreds of existing look and feels available to Swing programs. Many more look-and-feel packages are available from various sources.|
|Accessibility API|	Enables assistive technologies, such as screen readers and Braille displays, to get information from the user interface.|
|Java 2D API| Enables developers to easily incorporate high-quality 2D graphics, text, and images in applications and applets. Java 2D includes extensive APIs for generating and sending high-quality output to printing devices.|
|Internationalization |	Allows developers to build applications that can interact with users worldwide in their own languages and cultural conventions. With the input method framework developers can build applications that accept text in languages that use thousands of different characters, such as Japanese, Chinese, or Korean.|
