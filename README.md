VisualARQ Python API Documentation

This repository contains unofficial, community-generated documentation for the VisualARQ Python API, focused on scripting within Rhino (versions 7/8) and Grasshopper. VisualARQ is a flexible BIM tool for Rhino, and its scripting capabilities allow for automation of architectural elements like walls, beams, doors, windows, and more.

The API is accessed via the VisualARQ.Script module, which provides static methods for creating, modifying, and managing VisualARQ objects, styles, parameters, and project structures.

Note: This documentation is based on VisualARQ 3. Official documentation from Asuni (VisualARQ developers) may be limited or evolving—always cross-reference with their resources. This is not an official product and comes with no warranties.

Table of Contents
Installation and Setup
General Usage
API Overview
Examples
Contributing
License
Contact
Installation and Setup
Prerequisites:
Rhino 7 or 8 installed.
VisualARQ 3 plugin installed and licensed (available from visualarq.com).
Python scripting enabled in Rhino (built-in via EditPythonScript editor or Grasshopper's Python component).
Importing the API:
In Rhino's Python editor or Grasshopper: import VisualARQ.Script as va.
For GUID handling: from System import Guid.
No additional installation needed—the API is exposed via the VisualARQ DLL.
Testing Environment:
Use Rhino's EditPythonScript command for quick tests.
In Grasshopper, add a Python Script component and import as above.
Run dir(va) to list all available methods (as a starting point for exploration).
If you're setting this up in a GitHub repo, clone it and open the .py or .gh files in Rhino/Grasshopper.

General Usage
The following covers core concepts and best practices for using the API.

Version: VisualARQ 3 in Rhino 7/8

Notes:

All methods are static in VisualARQ.Script.
GUIDs are often used for IDs. Test in Rhino's EditPythonScript or Grasshopper Python component.
Import: import VisualARQ.Script as va.
From System import Guid; use Guid.Empty for checks.
Materials: Use Rhino's rs.AddMaterial() and indices for assignments.
Error Handling: Wrap calls in try/except for type mismatches. Many setters return Boolean (success/failure).
Profiles are shared across objects like beams, columns, railings, openings.
Opening methods are shared between doors and windows.
For a complete list of methods, see the initial dir(va) output.
Import: import VisualARQ.Script as va

Guids: From System import Guid; use Guid.Empty for checks.

Materials: Use Rhino's rs.AddMaterial() and indices for assignments.

Error Handling: Wrap calls in try/except for type mismatches. Many setters return Boolean (success/failure).

Profiles: Shared for beams, columns, railings, openings. Built-in templates (GetTemplate), custom via AddCurveProfileTemplate. Sizes set via SetProfileSize.

Styles: Common methods like GetStyleName, RenameStyle, DeleteStyle, GetAll*StyleIds.

Components: Use GetSubStyleComponents, GetParentStyleComponent, GetStyleComponentName, RenameStyleComponent, DeleteStyleComponent for sub-components (layers, leaves, etc.).

Parameters: Use AddDocumentParameter, AddObjectParameter, GetParameterValue, SetParameterValue for custom params.

Levels/Buildings: Use AddLevel, AddBuilding, GetLevelElevation, SetLevelElevation, etc., for multi-story management.

API Overview
The full API documentation has been split into separate files due to size. Each section covers a category of methods:

Object Creation and Management (e.g., AddWall, AddBeam, AddDoor; file: objects.md)
Style Management (e.g., AddWallStyle, SetWallStyleAlignment; file: styles.md)
Profile and Component Handling (e.g., AddProfileTemplate, GetProfileSize; file: profiles_components.md)
Parameters and Properties (e.g., AddParameter, SetParameterValue; file: parameters.md)
Project Structure (e.g., AddBuilding, AddLevel; file: project_structure.md)
Advanced Topics (e.g., Materials, Error Handling, Grasshopper Integration; file: advanced.md)
For the complete docs, refer to the files in the /docs folder of this repo. If you haven't uploaded them yet, organize them as Markdown or PDF files for easy reading.

Key Tip: All WallStyles should be created with no wrapping. This ensures clean, non-overlapping geometry in multi-segment walls—use methods like SetWallStylePathType to enforce straight or custom paths without automatic wrapping.

Examples
Here are basic code snippets to get started. Run these in Rhino's Python editor.

Creating a Wall Style
python

Collapse

Wrap

Run

Copy
import VisualARQ.Script as va
from System import Guid
import rhinoscriptsyntax as rs

# Add a new wall style
style_id = va.AddWallStyle("MyCustomWall")
if style_id != Guid.Empty:
    print("Wall style created:", style_id)
    # Set properties (no wrapping enforced)
    va.SetWallStyleAlignment(style_id, 0)  # Center alignment example
else:
    print("Failed to create wall style")
Adding a Parameter to an Object
python

Collapse

Wrap

Run

Copy
import VisualARQ.Script as va
from System import Guid

# Assume obj_id is a VisualARQ object GUID
obj_id = Guid("your-object-guid-here")
param_id = va.AddObjectParameter(obj_id, "CustomHeight", 1, "Height in meters")  # 1 = double type
va.SetParameterValue(param_id, obj_id, 3.5)
value = va.GetParameterValue(param_id, obj_id)
print("Custom Height:", value)
For more examples, see the examples/ folder (add your own scripts here).

Contributing
Contributions are welcome! If you have additions to the docs, fixes, or new examples:

Fork the repo.
Create a branch (git checkout -b feature/new-section).
Commit changes.
Open a Pull Request.
Please share any draft sections you have—I can incorporate them into the split files or expand this README. For instance, if you provide details on specific methods (e.g., beam or door APIs), I can add subsections here or create dedicated example files.

License
This documentation is released under the MIT License. See LICENSE for details.

Contact
For questions or feedback, reach out via GitHub Issues or [your contact info here]. Built with help from Grok by xAI.
