https://www.map2model.de/
https://www.youtube.com/watch?v=a0oo_HKDAmA
# Building and 3D Printing a Detailed 3D City Model of Tagum City

To build a detailed 3D model or 3D-printed layout of Tagum City—a 195.8 km² coastal capital in Davao del Norte with 23 barangays and 300,042 residents as of the 2024 census—you must partition the city by **spatial area** rather than arbitrary file size, pair open building and elevation datasets with automated tiling tools, and reserve drone photogrammetry for high-detail landmark blocks. For PC rendering, you cannot practically edit the entire 195.8 km² municipality in desktop software like Blender or SketchUp as one dense mesh; instead, you either work in fixed square map tiles (such as 1 km × 1 km blocks) or stream the city via an **OGC 3D Tiles** renderer (such as CesiumJS or deck.gl), which prioritizes tiles near the camera and swaps Levels of Detail (LOD) to keep memory consumption bounded. For 3D printing, partitioning by equal geographic area at a fixed physical scale ensures that individually printed tiles align and lock into a single contiguous layout.

All foundational data for Tagum City already exists, though no single free source provides surveyed heights and clean roofs for every structure. OpenStreetMap (OSM) supplies Tagum's road network and barangay boundaries, while global AI datasets—Google Open Buildings (v3 and 2.5D Temporal), Microsoft Global ML Building Footprints, and Overture Maps—fill missing footprints and provide 4 m raster height estimates. For terrain, Copernicus DEM GLO-30 provides immediate, free 30 m global coverage, and Philippine government agencies (UP TCAGP's LiPAD and NAMRIA) hold higher-resolution LiDAR and airborne IfSAR elevation datasets for the Tagum-Libuganon / Tagum River Basin upon formal request. Both mesh generation and tile slicing can be automated using Python scripts or chunked QGIS workflows. If you want realistic roofs and facades for focal landmarks via drone photogrammetry, Tagum sits roughly 40 km straight-line from Davao International Airport—well outside the 10 km airport exclusion zone—but flights over urban streets or any commercial survey still require strict compliance with Civil Aviation Authority of the Philippines (CAAP) rules under PCAR Part 11.

## Area Sizing, File Size, and PC Rendering Strategy

Large 3D city projects are organized spatially first, with polygon counts and file sizes managed _inside_ each tile rather than used as the cutting boundary. Splitting a city purely "per file size" (for example, slicing a master mesh every 100 MB) cuts buildings diagonally across uneven chunks, breaking UV coordinates, terrain skirts, and physical tile joints.

### Why You Partition by Area, Not Arbitrary File Size

Depending on whether you want a static desktop scene or an interactive city-wide map, spatial tiling controls PC workload in two distinct ways. In desktop editing tools like Blender, dense urban geometry multiplies quickly: Blosm documentation notes that a 2.3 × 1.5 km section of central Amsterdam imported with detailed Google 3D buildings exceeds 2.5 million triangles across more than 500 tile objects. Dividing Tagum into uniform metric grid squares (such as 1 km × 1 km or 2 km × 2 km tiles) keeps individual `.blend`, `.glb`, or `.stl` files light enough to edit and repair without exhausting system RAM.

For real-time PC or browser viewing across the full 195.8 km², streaming standards like **OGC 3D Tiles** pair spatial pyramids with **Level of Detail (LOD)** and file-size/error budgets. Distant barangays load as large, coarse tiles using simplified geometry and untextured monochrome or semantic-color shaders, while tiles close to the camera stream higher-detail geometry paired with baked texture atlases (which pack multiple building facades into one image per tile to cut GPU draw overhead). This architecture gives you smooth frame rates on standard PCs without loading all 195.8 km² at full resolution at once.

### Choosing Area Size and Scale for 3D Printing Tagum City

For a 3D-printed layout, your maximum area per file depends on your printer bed and chosen scale. Assuming a standard **200 mm × 200 mm (20 cm × 20 cm)** modular tile—which fits comfortably on common 220 × 220 mm or 256 × 256 mm FDM build plates—and a 0.4 mm nozzle requiring at least **0.8 mm** (two perimeters) for reliable thin walls, the trade-offs across Tagum's 195.8 km² municipal footprint and a 4 × 4 km (16 km²) urban core centered on the Magugpo barangays break down as follows:

|**Map Scale**|**Ground Area per 200 × 200 mm Tile**|**Tiles for 4 × 4 km Urban Core**|**Tiles for Full City (195.8 km²)**|**Min. Printable Feature (0.8 mm Wall)**|**Height of a 10 m (3-Story) Building**|**Best Use Case**|
|---|---|---|---|---|---|---|
|1:1,000|200 × 200 m (0.04 km²)|400 tiles|~4,895 tiles|0.8 m|10.0 mm|Single landmark, civic center, or school campus|
|1:2,000|400 × 400 m (0.16 km²)|100 tiles|~1,224 tiles|1.6 m|5.0 mm|Detailed downtown block series (Magugpo Poblacion)|
|1:2,500|500 × 500 m (0.25 km²)|64 tiles|~783 tiles|2.0 m|4.0 mm|Multi-block urban corridor display|
|1:5,000|1,000 × 1,000 m (1.00 km²)|16 tiles (4 × 4 grid, 80 × 80 cm)|~196 tiles|4.0 m|2.0 mm|Balanced Magugpo urban core wall or table display|
|1:10,000|2,000 × 2,000 m (4.00 km²)|4 tiles (2 × 2 grid, 40 × 40 cm)|~49 tiles (~7 × 7 grid, 1.4 × 1.4 m)|8.0 m|1.0 mm|Whole-city overview (requires building height/width scaling)|

At 1:10,000, any structure narrower than 8.0 m falls below the 0.8 mm wall threshold, and a 3 m single-story building prints only 0.3 mm tall. Printing only the central 4 × 4 km Magugpo core at **1:5,000** (16 tiles forming an 80 × 80 cm layout) keeps print time manageable while preserving street grids and buildings; applying a slight building scale boost (1.2×–1.5× is common in cityscape generators) or vertical exaggeration further protects fragile roofs.

## Available Geospatial and 3D Data for Tagum City

Tagum City spans elevations from 0 m along the Davao Gulf coast to 1,167 m at its highest inland peak, with the city center near 7.4468°N, 125.8095°E estimated at 24.0 m above sea level. You can assemble a 3D city model by combining open vector footprints, AI-derived height rasters, and satellite or airborne elevation models:

|**Dataset**|**Content & Resolution**|**Coverage & Tagum Availability**|**License & Access**|**Key Limitations for 3D Modeling**|
|---|---|---|---|---|
|OpenStreetMap (OSM)|Vector roads, waterways, land use, building polygons, height/level tags|Active in Tagum (e.g., 23 barangay boundaries; 30+ named roads recorded in San Miguel)|ODbL; free via Overpass API or QGIS QuickOSM|Building footprints and `height` / `building:levels` tags are often incomplete outside city centers|
|Google Open Buildings v3 & 2.5D Temporal|v3: 1.8B polygons (May 2023 inference); 2.5D: 4 m height raster (2016–2023)|Covers South & Southeast Asia (including Philippines; integrated by OneGeo in Feb 2026)|CC BY 4.0 or ODbL 1.0; via Earth Engine, S2 cells, or HDX|Closely packed tin-roofed buildings often merge into one polygon; v3 polygons lack height attributes|
|Microsoft Global ML Building Footprints|1.4B AI-detected footprints (Aug 2026 refresh)|Global zoom-9 quadkey `.csv.gz` files (imagery mostly 2014–2024)|CDLA Permissive 2.0; free via `dataset-links.csv`|Heights provided only on select subsets; omits newer construction|
|Overture Maps (Buildings)|Conflated OSM footprints filled with Google/Microsoft polygons + available heights|Global coverage|Open data; via Overture CLI or QGIS plugin|Inherits missing height attributes where underlying sources lack them|
|Copernicus DEM GLO-30|30 m Digital Surface Model (TanDEM-X InSAR, 2011–2015)|Global coverage including Tagum|Free global license; OpenTopography, Copernicus Data Space, AWS S3|<4 m vertical accuracy (90% LE) and 1.68 m global RMSE; as a DSM, it includes tree canopies and roofs|
|Phil-LiDAR 1 & 2 (DOST-UP DREAM)|1 m LiDAR DTM and DSM (demonstrated in regional floodplains)|Technical reports reference Tagum and Davao del Norte; verify exact urban tile coverage via AOI check|Manual request via `lipad.dream.upd.edu.ph` (letter + AOI shapefile); commercial clearance via `bdo@dream.upd.edu.ph`|Automated portal under maintenance (uses Google Forms); administrative processing wait|
|NAMRIA Airborne IfSAR & 1:10,000 DEM|Airborne IfSAR DEM and upgraded 1:10,000-scale DEM|eFOI and JICA records document DEMs for the Tagum-Libuganon River Basin|Philippine government request (NAMRIA / eFOI)|Requires formal request; exact pixel spacing is not published in those public summaries|
|Google Photorealistic 3D Tiles|Textured 3D photogrammetry mesh (OGC 3D Tiles)|2,500+ cities across 49 countries; Davao City has 3D data, while **Tagum coverage is unverified**|Google Maps Platform (1,000 free Blosm imports/mo) or Cesium ion|Must verify in Google Earth's "3D coverage" layer; ToS prohibits extracting the mesh for resale or standalone reuse|

Because no single dataset covers every building cleanly, blending them gives the best result. Start with **Overture Maps** or **OSM** for streets, waterways like the Libuganon River, and mapped structures, then sample heights from **Google Open Buildings 2.5D Temporal** (or a pre-conflated dataset incorporating it for the Philippines) wherever OSM height tags are missing. Be aware of a documented computer-vision pitfall in dense Asian and African neighborhoods: Terra Lab's independent benchmark in Dandora, Nairobi found that only 16% of Google polygons matched OSM at >0.5 Intersection-over-Union, largely because adjacent tin-roofed structures merged into single large polygons (up to 62 OSM buildings inside one Google polygon). Tracing focal city blocks manually over satellite imagery—or mapping them by drone—eliminates these merged footprints.

For terrain, start immediately with **Copernicus DEM GLO-30**; its radar acquisition penetrates tropical cloud cover that degrades optical DEMs like ALOS AW3D30, and it corrects the voids and outdated 2000-era elevation of SRTM. If you need sharper riverbanks and road embankments for flood-prone or coastal Tagum barangays, submit an Area-of-Interest (AOI) shapefile to UP TCAGP's LiPAD forms or NAMRIA for LiDAR or IfSAR coverage.

## Software Workflows for Digital Models and 3D Printing

Digital rendering pipelines prioritize visual textures and camera streaming, whereas 3D printing pipelines require **watertight (manifold)** solid geometry—completely closed volumes with flat bottoms, outward normals, and no non-planar faces or internal gaps. An architectural 3D printing guide citing a 2016 study by Biljecki, Ledoux, and colleagues across 37 official city datasets (3.6 million buildings) noted that **none** were free of geometric errors, mostly due to subtle non-planar polygons and open bottom faces that break slicers. Use workflows tailored to your end output rather than sending raw GIS files straight to a printer.

### Digital Rendering Pipelines (PC and Browser)

1. **Blender + Blosm (formerly Blender-OSM):** Best for architectural visualization, flythroughs, and stylized renders. Blosm downloads OSM buildings (extruding by height/floors and generating roof shapes), roads with widths, water bodies, and ~30 m terrain in a few clicks. The paid Pro version adds procedural UV-mapped facades, trees, and texture baking. If Google Earth confirms 3D coverage over parts of Tagum, Blosm can also import Google 3D Tiles directly.
    
2. **QGIS + Qgis2threejs:** Best for quick GIS-to-web 3D presentations. Load your DEM, imagery, and building footprints in QGIS and export an interactive browser scene or **glTF** file. Note that `Qgis2threejs` **removed STL, OBJ, and COLLADA export in version 2.0**, so modern versions output glTF rather than ready-to-print STLs.
    
3. **CesiumJS or deck.gl (`Tile3DLayer`):** Best for exploring the full 195.8 km² Tagum map smoothly in a browser or desktop app. Convert your tiled buildings and terrain into **OGC 3D Tiles** via Cesium ion or a custom tiling pipeline so the viewer dynamically streams only visible tiles.
    

### 3D Printing Pipelines (Watertight STL and Multi-Color 3MF)

1. **Mini Skyline (`miniskyline.com`) or Self-Hosted `map2model`:** The original browser tool `map2model.com` shut down by October 2026 following legal notices, and its shutdown notice recommends **Mini Skyline** first (which also opens saved Map2Model `project.json` files). Mini Skyline runs free in the browser without an account, combines OSM buildings, roads, water, parks, and real terrain, splits large cities into equal-scale tiles, and exports multi-color **3MF** files pre-separated by feature for Bambu Studio, PrusaSlicer, or Cura (convertible to STL in your slicer). Separately, an open-source self-hosted project (`lxie-leo/map2model` on GitHub) exports 3D white models and terrain across 17 formats for Blender, AutoCAD, QGIS, and Cesium.
    
2. **QGIS + DEMto3D:** When turning a GeoTIFF DEM (Copernicus GLO-30 or Phil-LiDAR) into printable terrain, use the **DEMto3D** plugin rather than Qgis2threejs; DEMto3D generates a watertight STL complete with solid side walls and a flat base.
    
3. **Cadmapper, TopoExport, and Touch Mapper:** **Cadmapper** exports organized CAD/SketchUp files free up to 1 km² (1 map per month; 1–100 km² paid with academic discounts), which you solidify in SketchUp using _Eneroth Terrain Volume_ or in Blender. A competitor comparison by `osm2cdr.com` (checked September 2026) lists **TopoExport** with a free BASIC tier up to 1 km² (2 exports/day at 5 m terrain resolution) and paid PRO/PRO+ tiers up to 10–50 km² (0.1 m terrain). **Touch Mapper** generates free, high-contrast tactile STLs centered on any address.
    
4. **Interlocking Tile Systems:** To mount multiple 200 × 200 mm Tagum tiles together without glue alignment errors, design or attach a 4 mm-thick base skirt with chamfered zipper joints (such as open 3D-printed interlocking tile systems on MakerWorld) so tiles lock mechanically across all axes.
    

## Automating the City-Scale Pipeline

If you plan to print a 16-tile Magugpo grid or render dozens of barangay tiles, manual bounding-box downloads become tedious and error-prone. You can automate extraction, extrusion, and tiling using three established approaches:

1. **Automated Python Vector-to-STL Pipeline (`OSMnx` + `Shapely` + `Trimesh`):** Write a Python script that generates a uniform grid of bounding boxes over Tagum City in a metric projection. For each cell, `OSMnx` fetches or clips cached OSM buildings and roads; `Shapely` buffers road lines into solid printable strips and repairs geometry; a height lookup pulls OSM `height` tags, floor estimates, or sampled Google 2.5D raster values; and `Trimesh` extrudes each polygon onto a solid rectangular baseplate and exports a watertight tile STL.
    
2. **Chunked City-Scale Downloads in QGIS (`osm_quick_3d`):** Single Overpass API queries often fail across a 195.8 km² city. The QGIS plugin `osm_quick_3d` automatically splits areas up to 2,500 km² into compact 100 km² Overpass requests, caches each chunk independently, and merges them without seams—allowing batch processing over a uniform grid.
    
3. **Self-Hosted Web Task Automation (`lxie-leo/map2model`):** Running the open-source FastAPI/Vue3 `map2model` stack locally lets you queue bounding boxes, pull OSM plus AWS terrain tiles, and export layered `.glb` files (with preserved `TERRAIN` and `BUILDING` node names) for batch import into Blender or Cesium.
    

## Drone Photogrammetry for Custom Buildings and Neighborhoods

Public GIS datasets produce simple flat-roofed extrusions ("LoD1" block models). To capture true architectural rooflines, facades, signage, and trees for key Tagum landmarks—such as Tagum City Hall, churches, malls, or subdivisions—**drone photogrammetry** (Structure from Motion) reconstructs textured 3D meshes directly from overlapping aerial photographs.

### Mission Planning and Photogrammetry Workflow

1. **Fly a Crosshatch (Double-Grid) Pattern with Oblique Angles:** Use an automated flight planner such as DJI Pilot 2, Pix4Dcapture, DroneDeploy, or Litchi. Fly two perpendicular grids rather than a single straight-down (nadir) pass, and tilt the camera obliquely (around 45°, or at least 10–20° forward if flying a single pass) so vertical building facades are clearly photographed. A peer-reviewed _Applied Sciences_ test using DJI Phantom 4 RTK aircraft showed that combining nadir and oblique imagery reduced vertical (Z) errors by five times compared with nadir-only flights.
    
2. **Set Overlap, Altitude, and Speed:** Program **80% frontal overlap and 70–80% side overlap** (higher over dense vegetation or uniform roofs). Fly at **60–120 m AGL** to achieve a **1–3 cm/pixel Ground Sampling Distance (GSD)**, defined as `(sensor width × altitude) / (focal length × image width in pixels)`—for example, a 45 MP DJI Zenmuse P1 yields ~1.3 cm/px at 60 m and ~2.6 cm/px at 120 m per a vendor accuracy guide. Keep speed below 10 m/s, and slow to **1.5–2.0 m/s** on rolling-shutter consumer drones (such as the DJI Mini series) to minimize line-by-line geometric skew. Fly under overcast skies whenever possible, because direct sunlight casts sharp moving shadows that confuse feature matching.
    
3. **Georeference with RTK/PPK and Ground Control Points (GCPs):** Standard consumer drone GPS drifts by 2–5 m. To achieve **1–3 cm horizontal and 2–3 cm vertical accuracy**, use an RTK/PPK-enabled drone or distribute **5–8 GCPs** across the site perimeter and interior (plus 2–4 independent checkpoints) surveyed with RTK GNSS or a total station. Even with RTK drones, independent checkpoints validate vertical alignment against your city terrain.
    
4. **Reconstruct the Mesh:** Process your images in **OpenDroneMap / WebODM** (free self-hosted via Windows/macOS installers or Docker; cloud WebODM Lightning from $24/month billed yearly or $29 monthly), **Agisoft Metashape Standard** ($179 one-time license per software comparison listings), or **RealityScan** (free for individuals, students, educators, and businesses under $1 million USD annual revenue; $1,250/seat/year above that). Export the orthomosaic, DSM, and textured `.obj`/`.glb` mesh.
    
5. **Simplify and Make Watertight Before Merging:** Raw photogrammetry meshes are often too heavy for SketchUp and contain floating sky noise or open edges. Clean, decimate, and run 3D-print manifold repair in Blender's 3D-Print extension, then cut the building's footprint out of your base city tile and drop the scaled landmark mesh into place.
    

### Philippine Drone Laws (CAAP PCAR Part 11) in Tagum City

In the Philippines, drone flights are regulated by CAAP under **PCAR Part 11** (including Memorandum Circular 21-14) and **Republic Act No. 10173 (Data Privacy Act of 2012)**. Tagum's center lies approximately **40.0 km** straight-line (41–47 km by road) from Davao International Airport (DVO), placing the city comfortably outside CAAP's 10 km airport buffer. However, urban mapping inside Tagum still triggers strict populated-area and commercial rules:

|**Flight Category**|**Drone Weight / Purpose**|**Required CAAP Credentials**|**Populated Area / >400 ft / Night Operations**|
|---|---|---|---|
|Recreational / Personal|Under 7 kg, strictly non-commercial|None|**Prohibited** without Special Flight Permit; stay clear of crowds, markets, schools, urban streets, and government/military facilities|
|Large Recreational|7 kg or heavier|RPAS Registration + RPA Controller Certificate (+ airworthiness/approval for large UAVs)|Special Flight Permit required|
|Commercial (Paid, Client, or Business Survey)|Any weight (even sub-250 g drones)|RPAS Registration (₱1,500 + 12% VAT), RPA Controller Certificate (≥5 hrs training, exam, checkride; ₱3,360 fee), and RPAS Operator Certificate (ROC with ops manual & liability insurance)|**Special Flight Permit** mandatory for flying over populated urban areas, >400 ft (122 m) AGL, BVLOS, or at night|

Without a Special Flight Permit, all pilots must keep flights in daylight, within direct visual line of sight, below 400 ft (~122 m) AGL, and at least 30 m away from uninvolved persons. Violations carry aircraft grounding or confiscation, license suspension, and **₱20,000 to ₱100,000 in fines per violation**, while photographing identifiable people inside private residential yards without consent risks Data Privacy Act liability. To map dense Tagum urban landmarks legally, coordinate with the Tagum City LGU and engage a CAAP-certificated RPAS operator holding a Special Flight Permit, or limit unpermitted hobbyist mapping strictly to uninhabited parcels well clear of populated streets and bystanders.