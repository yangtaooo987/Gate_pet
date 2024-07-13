# Gate_pet

## structure of the simulation

### data
数据库
```
##########################################################
# Material Database
##########################################################
/gate/geometry/setMaterialDatabase data/GateMaterials.db
```


### output
root输出
intefile 输出

### mac
Geometry
#### word

```
# World
/gate/world/geometry/setXLength 1.5 m
/gate/world/geometry/setYLength 1.5 m
/gate/world/geometry/setZLength 1.5 m
/gate/world/vis/setColor white
/gate/world/vis/forceWireframe
/gate/world/setMaterial Air
```


#### system
pet
spect
<details>
<summary>pet head</summary>

```
#	CYLINDRICAL
/gate/world/daughters/name cylindricalPET
/gate/world/daughters/insert cylinder
/gate/cylindricalPET/placement/setTranslation 0.0 0.0 0.0 cm
/gate/cylindricalPET/geometry/setRmax 52.0 cm
/gate/cylindricalPET/geometry/setRmin 39.9 cm
/gate/cylindricalPET/geometry/setHeight 40.2 cm
/gate/cylindricalPET/setMaterial Air
/gate/cylindricalPET/vis/forceWireframe
/gate/cylindricalPET/vis/setColor white

#	HEAD
/gate/cylindricalPET/daughters/name head
/gate/cylindricalPET/daughters/insert box
/gate/head/placement/setTranslation 44.0 0.0 0.0 cm
/gate/head/geometry/setXLength  8.0 cm
/gate/head/geometry/setYLength 32.0 cm
/gate/head/geometry/setZLength 40.2 cm
/gate/head/setMaterial Air
/gate/head/vis/setVisible 0


#	MODULE
/gate/head/daughters/name module
/gate/head/daughters/insert box
/gate/module/placement/setTranslation 2.5 0.0 0.0 cm
/gate/module/geometry/setXLength 3.0 cm
/gate/module/geometry/setYLength 8.0 cm
/gate/module/geometry/setZLength 10.0 cm
/gate/module/setMaterial Air
/gate/module/vis/setVisible 0


#	BLOCK
/gate/module/daughters/name block
/gate/module/daughters/insert box
/gate/block/placement/setTranslation 0.0 0.0 0.0 cm
/gate/block/geometry/setXLength 30 mm
/gate/block/geometry/setYLength 15.9 mm
/gate/block/geometry/setZLength 19.9 mm
/gate/block/setMaterial Air
/gate/block/vis/setVisible 0

#	C R Y S T A L
/gate/block/daughters/name crystal
/gate/block/daughters/insert box
/gate/crystal/placement/setTranslation 0.0 0.0 0.0 cm
/gate/crystal/geometry/setXLength 3.0 cm
/gate/crystal/geometry/setYLength 3.0 mm
/gate/crystal/geometry/setZLength 3.8 mm
/gate/crystal/setMaterial Air
/gate/crystal/vis/setVisible 0


#	LSO layer
/gate/crystal/daughters/name LSO
/gate/crystal/daughters/insert box
/gate/LSO/placement/setTranslation 0.0 0.0 0.0 cm
/gate/LSO/geometry/setXLength 3.0 cm
/gate/LSO/geometry/setYLength 3.0 mm
/gate/LSO/geometry/setZLength 3.8 mm
/gate/LSO/setMaterial LSO
/gate/LSO/vis/setColor green


#	R E P E A T    C R Y S T A L
/gate/crystal/repeaters/insert cubicArray
/gate/crystal/cubicArray/setRepeatNumberX 1
/gate/crystal/cubicArray/setRepeatNumberY 5
/gate/crystal/cubicArray/setRepeatNumberZ 5
/gate/crystal/cubicArray/setRepeatVector 0.0 3.2 4.0 mm


#	R E P E A T    BLOCK
/gate/block/repeaters/insert cubicArray
/gate/block/cubicArray/setRepeatNumberX 1
/gate/block/cubicArray/setRepeatNumberY 5
/gate/block/cubicArray/setRepeatNumberZ 5
/gate/block/cubicArray/setRepeatVector 0.0 1.6 2.0 cm

#	R E P E A T MODULE
/gate/module/repeaters/insert cubicArray
/gate/module/cubicArray/setRepeatNumberX 1
/gate/module/cubicArray/setRepeatNumberY 4
/gate/module/cubicArray/setRepeatNumberZ 4
/gate/module/cubicArray/setRepeatVector 0.0 8.0 10.0 cm

#	R E P E A T HEAD
/gate/head/repeaters/insert ring
/gate/head/ring/setRepeatNumber 8

/gate/cylindricalPET/moves/insert orbiting
/gate/cylindricalPET/orbiting/setSpeed .1875 deg/s
/gate/cylindricalPET/orbiting/setPoint1 0 0 0 cm
/gate/cylindricalPET/orbiting/setPoint2 0 0 1 cm

#	A T T A C H    S Y S T E M 
/gate/systems/cylindricalPET/rsector/attach head
/gate/systems/cylindricalPET/module/attach module
/gate/systems/cylindricalPET/submodule/attach block
/gate/systems/cylindricalPET/crystal/attach crystal
/gate/systems/cylindricalPET/layer0/attach LSO

#	A T T A C H    C R Y S T A L  SD

/gate/LSO/attachCrystalSD

/gate/systems/cylindricalPET/describe

```
</details>

#### phantom
```
# Define a cylindrical box
/gate/world/daughters/name         phantom
/gate/world/daughters/insert       cylinder
/gate/phantom/geometry/setRmin     0.0 cm
/gate/phantom/geometry/setRmax     10.0 cm
/gate/phantom/geometry/setHeight   70. cm
/gate/phantom/setMaterial          Water
/gate/phantom/vis/setColor         blue

# Define the sensitive detector
/gate/phantom/attachPhantomSD
#/gate/endshielding/attachPhantomSD
#/gate/septa/attachPhantomSD

```
#### detectors

#### digitizer
<details>
<summary>pet digitizer</summary>

```

# The singles
/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/insert                        adder
#/gate/digitizerMgr/BGO/SinglesDigitizer/Singles/insert                        adder

#/gate/digitizerMgr/BGO/SinglesDigitizer/Singles/insert                        merger
#/gate/digitizerMgr/BGO/SinglesDigitizer/Singles/merger/addInput  adder/Singles_LSO

/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/insert                        readout
/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/readout/setDepth              1

/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/insert                        energyResolution
/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/energyResolution/fwhm        0.26
/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/energyResolution/energyOfReference 511. keV

/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/insert                        energyFraming
/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/energyFraming/setMin      350. keV
/gate/digitizerMgr/LSO/SinglesDigitizer/Singles/energyFraming/setMax            650. keV


# Coincidence sorter
/gate/digitizerMgr/CoincidenceSorter/Coincidences/setInputCollection Singles_LSO
/gate/digitizerMgr/CoincidenceSorter/Coincidences/setWindow          120 ns
/gate/digitizerMgr/CoincidenceSorter/Coincidences/MultiplesPolicy    takeWinnerOfGoods

/gate/digitizerMgr/name                            delay
/gate/digitizerMgr/insert                          CoincidenceSorter
/gate/digitizerMgr/CoincidenceSorter/delay/setInputCollection Singles_LSO
/gate/digitizerMgr/CoincidenceSorter/delay/setWindow                 120 ns
/gate/digitizerMgr/CoincidenceSorter/delay/setOffset                 500 ns
#/gate/digitizerMgr/delay/MultiplesPolicy          takeWinnerOfGoods

```

</details>

#### physics，cuts

```
/gate/physics/addPhysicsList emstandard_opt3

# High cut by default
/gate/physics/Gamma/SetCutInRegion      world 1 km
/gate/physics/Electron/SetCutInRegion   world 1 km
/gate/physics/Positron/SetCutInRegion   world 1 km

# Cuts for particle in NEMACylinder
/gate/physics/Gamma/SetCutInRegion      phantom 1.0 mm
/gate/physics/Electron/SetCutInRegion   phantom 1.0 mm
/gate/physics/Positron/SetCutInRegion   phantom 1.0 mm

# Cuts for particle in LSO
/gate/physics/Gamma/SetCutInRegion      LSO 1.0 mm
/gate/physics/Electron/SetCutInRegion   LSO 1.0 mm
/gate/physics/Positron/SetCutInRegion   LSO 1.0 mm

# Cuts for particle in BGO
#/gate/physics/Gamma/SetCutInRegion      BGO 1.0 mm
#/gate/physics/Electron/SetCutInRegion   BGO 1.0 mm
#/gate/physics/Positron/SetCutInRegion   BGO 1.0 mm


/gate/physics/processList Enabled
/gate/physics/processList Initialized
```
### initialization
[!NOTE]
The initialisation step must be performed after the geometry, phantom and digitizer is set and before the definition of the source and root output:

#### head

#### 

