# Calculate band structure with yambopy
> _required_: yambopy and abipy(for interpolation)

Calculating the band structure from `QE` with yambopy is simple and straightforward.

Those are the necessary steps:
1) run a `scf` calculation with `QE`
2) run a `bands` or `nscf` calculation with `QE`

Here's is a simple python script that reads from the `.xml` file of a `QE.save` database

```python
# Define path in reduced coordinates using Class Path
npoints = 30
path_kpoints = Path([ [[  0.0,  0.0,  0.0],'$\Gamma$'],
              [[  0.5,  0.0,  0.0],'M'],
              [[1./3.,1./3.,  0.0],'K'],
              [[  0.0,  0.0,  0.0],'$\Gamma$']], [npoints,npoints,npoints] )

def run_plot(p):
    print("running plotting:")
    xml = PwXML(prefix=prefix,path='PathToQeSave')
    xml.plot_eigen(p) # return a fig object that one can edit

run_plot(path_kpoints)
```

Assuming that the calculation has been performed with the same path as `path_kpoints`. Familiarizing with the source code is mandatory to create custom plot. For example one could either call for `plot_eigen_ax` method in `PwXML`.

Examples are available in the `databases_qepy.tar` file which you can extract with `tar -xvf databases_qepy.tar`

In particular, one of the example is for [Fe-metal](#fe-metal)

## Fe-metal
Run the script with
```python
python plot-qe-bands.py
```
and visualize the result. The code goes into the `.save` folder of QE and creates a `PwXML` class object. At the end you get a plot of the band structure with spin up and down