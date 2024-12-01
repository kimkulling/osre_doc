.. _osre_Coding_Conventions_cpp:

================================
Coding conventions
================================

How to document the code
========================
::

  /// \brief          A brief one or two line description of the function.
  /// \note           An important note the user should be aware of--perhaps many 
  ///                 lines.
  /// \details        Extra details.
  ///                 Perhaps
  ///                 even
  ///                 a long
  ///                 paragraph.
  /// \param[in]      var1            Description of variable one, an input
  /// \param[in]      var2            Description of variable two, an input
  /// \param[out]     var3            Description of variable three, an output 
  ///                     (usu. via a pointer to a variable)
  /// \param[in,out]  var4            Description of variable four, an 
  ///                     input/output (usu. via a pointer) since its initial 
  ///                     value is read and used, but then it is also updated by 
  ///                     the function at some point
  /// \return         Description of return types. It may be an enum, with these
  ///                 possible values:
  ///                 - #ENUM_VALUE_1
  ///                 - #ENUM_VALUE_2
  ///                 - #ENUM_VALUE_3

::
