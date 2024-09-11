# WMM_Tiny

This is a small embedded C99 implementation of the World Magnetic Model published by NOAA for calculating the magnetic field at any point on the world's surface for a given date in the years 2020 to 2025. This implementation uses the latest coefficients from NOAA for the years 2020 to 2025.

This implementation is a cut-down version of the source code supplied by NOAA and is intended for small embedded systems. 

## Limitations

The limitations of this project are:

- **Calculates magnetic variation only**.
- **Altitude**: Only calculates the magnetic variation at altitude zero relative to the WGS84 ellipsoid, which is approximately the Earth's surface.
- **Precision**: Uses floats in its calculations, not doubles. This will reduce accuracy very slightly.
- **Coefficient Storage**: Stores the coefficients in a compressed format. The floating point coefficients are converted to fixed point floats and then stored as scaled variable-length signed integers.

## Project Intent

The intention of this project is to reduce the code space as much as possible at the expense of RAM. This makes it suitable for small embedded processors short on code memory but with 4.2 kBytes of RAM available. 

The coefficients are compressed in code but are expanded back into their normal format in RAM when the code runs.

## Necessary Files

The following files are necessary for porting:

- `inc/wmm.h`
- `src/wmm.c`
- `src/WMM_COF.c`

Doxygen style documentation of the API is found in `wmm.h`. Example code calling the API is found in `Core/Src/main.c`.

> **Memory Usage**: This code requires 4.2 kBytes of RAM permanently. All memory is statically allocated.

## Code Structure

The source code may look a bit strange to seasoned C programmers, with `goto` statements present. This is because this is a port of the WMM source code provided by NOAA, which is itself a port of the original WMM code written in Fortran.

## WMM_COF Converter Sub-project

Under the folder `wmm_cof_converter` is a sub-project that compresses the coefficients file supplied by NOAA (`WMM.COF`) into a C99 source file that is used by the WMM_Tiny project (`WMM_COF.c`).

- The project includes the version of `WMM.COF` current at the time of writing. 
- You should check NOAA's website for a later updated version and regenerate `WMM_COF.c` if a later version is available.
  
This sub-project also has project files allowing it to be loaded and built in STM32CubeIDE, or the single C99 source file `wmm_cof_converter.c` can be compiled by any C99 compiler.

## Original Source

The original code was developed by NOAA / NCEI. For more details, visit the official website:  
[https://www.ngdc.noaa.gov/geomag/WMM/](https://www.ngdc.noaa.gov/geomag/WMM/)

## Usage Example

Here is a basic example of how to use the API:

```c
wmm_init();
float wmm_date = wmm_get_date(20U, 4U, 16U);
float variation;
E0000(52.88972f, -4.413889f, wmm_date, &variation);
