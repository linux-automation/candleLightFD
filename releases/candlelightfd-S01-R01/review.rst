PCB / PCBA Review Checklists
============================

Schematic Related Topics
########################

Allgemeines
-----------

* [ ] Braucht der Reset-Pin einen Pull-Up?
* [ ] Testpunkte für RTS, CTS, falls da genug Platz ist?


Drawing
-------

* [x] Are all sheets drawn with the same paper size? Is A4 used if possible?
* [x] Does the title block contain the name of the used git-repository?
* [x] Is the revision count in the title block up to date?
* [x] Are comments added where the schematic is not intuitive?


Nets and hierarchy
-------------------

* [x] Are all mayor nets named? (Either with a net label or due to the use of buses or hierarchical sheet entries.)
* [x] Are the directions of hierarchical sheet entries set in a meaningful way?


ERC
----

* [x] Does an ERC return errors? Are all severe error commented in the schematic?


Meta-Information
----------------

* [x] Do all symbols of manufacturer parts contain a "Manufacturer" and "MPN" field?
* [x] Are all libraries used from a local checkout of the kicad-ptx-lib or
      a projekt-local library? (Do not use absolute or system-wide paths.)


Electrical
----------

* [x] Do all Capacitors have a sufficient voltage derating applied?
      - Electrolyte Capacitors: U_R >= U_max * 1.3
      - Ceramic Capacitors:     U_R >= U_max * 1.5 .. 2
* [x] For all variants:
      * Are all necessary parts placed?
      * Are unnecessary parts not placed? (fit: none)
* [x] Are all analog devices (LDOs, OP-AMPs,...) within a stable operating region?
      * Check datasheet for stability region.
      * Check capacitive loads.
      * Check current to reference pins is sufficient
* [-] For designs including step-up/-down or flyback magnetics:
      are there footprints onto which snubber networks can be placed?
* [x] For designs containing digital high speed clock traces:
      are there footprints onto which filters can be placed?

PCB
####

General
-------

* [x] Does the PCB contain a PTXguin or a LAG rocket-penguin if the
      design is made for PTX or the LAG.
* [x] Does the PCB has a layer stackup in copper on every layer?
* [x] Is the PCB marked with the PCB number in copper on at least one side?
      Additional Marks in Mask or Silkscreen are possible.
      Marking must use variables to create the following names:
      * prefix
      * type_number
      * release
* [x] Does a DRC return no severe errors?
* [x] Are all libraries used from a local checkout of the kicad-ptx-lib or
      a projekt-local library? (Do not use absolute or system-wide paths.)
* [ ] Have all changes been imported from the schematic?
* [x] Is the auxiliary origin reference point set to a corner of the PCB?
      Preferably on the metric grid.
      Preferably on the lower left corner of the PCB.

* [x] Is the layer stack in the drawing frame up to date?
      - All layers mentioned?
      - Copper thickness, impedances, finishes correct?

* [x] Are fiducials placed on all sides with SMD or THT components?
      Place at least 3 fiducials on each side.
      Does the fiducial match the manufacturers specifications?

* [-] Are planes split in a useful way?
      - Analog and digital parts are separated
      - High currents are directed away from sensitive circuits
      - Oscillators are placed on a GND-island
* [-] Are the reference planes of high-speed signals contiguous?
      Are planes AC-coupled where reference planes change?

* [-] Have impedance controlled traces been routed with the correct width/gap for the layer?
* [-] Is a definition of impedance controlled traces placed on a non-copper layer
      and is it up to date?

* [-] Are clearance and creepage requirements (for areas with high voltages)
      met?

Testing
#######

* [x] Is there a way to bring up the device at EOL or in maintenance?
      e.g. flash software, readout images, ...
      All signals needed to bring the device into non-normal
      boot-modes must be exposed to a connector or testpoint.
* [x] Is there a way to test all mayor functions at EOL?
* [-] Does the PCB have drillings for mounting on a testing rig?
* [-] Do all mayor nets have test points or vias without tenting?
* [-] Are all test points placed at least 2.54mm apart?

* [x] Are potentially noisy traces (e.g. unfiltered outputs of voltage
      regulators) only as wide as they have to be?
      Unnecessarily thick traces increase the coupling of noise onto
      planes and other traces.

Create release
##############

* [ ] Have Vias been untented? (jma's KiCAD-plugin: https://gitlab.pengutronix.de/jma/kicad_plugins)
* [ ] Have Planes been rebuilt? (use shortcut B in PCBNEW)

* [ ] Change Status from Draft to Release
* [ ] Create new folder in release
* [ ] Add release to release management (https://gitlab.pengutronix.de/hardware/release_management)


Manufacturing Data
##################

Directory structure is defined here:
  https://wiki.pengutronix.de/manual/devel/hardware/kicad-project.html

* [ ] Call `release_generator.py <release_tag>`

* Assembly:
  * [ ] Convert the -extended.bom.csv to .xlsx for manufacturing.

  * ( ) Optional: List of BOM changes:
        * [ ] Use tools/bom/bom_diff.sh prev-bom.csv cur-bom.csv | aha > bom-diff.html
              to generate a list of changes to the BOM between revisions.
        * [ ] Sanity-Check the listed changes to the bom.

  * [ ] Interactive HTML BOM:
        PCBNEW -> Interactive HTML BOM
        Settings:
        * Activate: "Incldue Tracks/zones"
        * Activate: "Include nets"

* PCB:
  * PCB Data as PDF:
    * Output into a directory like:
      ./release/name-S01-R01/name-P01-R01-V01/pdf/
    * PCBNEW -> File -> Plot
    * Settings:
      * Plot Format: Gerber
      * Active "Plot border and title block"
      * Deactivate "Exclude PCB edge from other layers"
      * Deactivate "Do not tent vias"
      * Activate "Check zone files before plotting"
      * Layers:
        * [ ] Cu-Layers depending on Project
        * [ ] Paste
        * [ ] Silkscreen
        * [ ] Mask
        * [ ] Uwgs. User (if containing e.g. Size of PCB)
        * [ ] Edge.Cuts
        * [ ] Fab and CrtYrd -layer
  * 3D Render of PCB as JPG:
    * Output into a directory like:
      ./release/name-S01-R01/name-P01-R01-V01/
    * PCBNEW -> 3D-View -> File -> 'Export Current ciew as JPEG'
    * Settings:
      * Set Silk and Mask color according to layer stack
    * [ ] One Render that shows the PCBA from TOP
    * [ ] One Render that shows the PCBA from BOT
    * ( ) Optionally: Renderings that show important details
  * [ ] Test-Data for flying probe:
    * Output into a directory like:
      ./release/name-S01-R01/name-P01-R01-V01/cad/
    * \*.cad
    * In PCBNEW -> File -> Export -> GenCAD
