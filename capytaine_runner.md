```python
import logging

import numpy as np
from capytaine import Mesh
from numpy import pi

import capytaine as cpt
from capytaine.meshes.geometry import Plane

from capytaine.meshes.geometry import xOz_Plane
from capytaine.meshes.symmetric import ReflectionSymmetricMesh

logging.basicConfig(level=logging.INFO, format='%(levelname)-8s: %(message)s')
bem_solver = cpt.BEMSolver()

waterdepth = 5.6



def make_database(body, omegas, wave_directions):
    # SOLVE BEM PROBLEMS
    problems = []
    for wave_direction in wave_directions:
        for omega in omegas:
            problems += [cpt.RadiationProblem(omega=omega, body=body, radiating_dof=dof, sea_bottom=-waterdepth) for dof in body.dofs]
            problems += [cpt.DiffractionProblem(omega=omega, body=body, wave_direction=wave_direction, sea_bottom=-waterdepth)]
    results = [bem_solver.solve(problem) for problem in problems]
    *radiation_results, diffraction_result = results
    dataset = cpt.assemble_dataset(results)

    dataset['diffraction_result'] = diffraction_result

    return dataset


if __name__ == '__main__':
    data = [
        ("P70", 75, 23.5, 4.5, 1.2),
        ("P70", 75, 23.5, 4.5, 2.0),
        ("P70", 75, 23.5, 4.5, 3.0),
        ("P70", 75, 23.5, 4.5, 3.5),
        ("P80", 84, 23.5, 5.5, 1.5),
        ("P80", 84, 23.5, 5.5, 2.0),
        ("P80", 84, 23.5, 5.5, 3.0),
        ("P80", 84, 23.5, 5.5, 4.0),
        ("P100", 100, 33, 7.6, 1),
        ("P100", 100, 33, 7.6, 1.5),
        ("P100", 100, 33, 7.6, 3),
        ("P100", 100, 33, 7.6, 4),
    ]

    for name, L, W, D, T in data:

        file_grid = r"C:\Users\beneden\Jottacloud\RdBr\Projects\HEBO Barges\barge_{}_T{}.stl".format(name, 10 * T)

        # file_grid = r"C:\Users\beneden\Jottacloud\RdBr\Projects\HEBO Barges\grid.stl"
        name = "Barge"

        file_out_nc = file_grid + '.nc'
        outfile = "{}.dhyd".format(file_grid[:-4])

        periods = np.array([1,2,3,3.5,4,4.5,5,5.5,6,7,8,10,12,14,16,18,20,25,])
        omega = 2*np.pi / periods

        directions = np.linspace(0,pi,9)


        boat = cpt.FloatingBody.from_file(file_grid, file_format="stl", name=name)

        boat.keep_immersed_part()

        mesh = ReflectionSymmetricMesh(boat.mesh, plane=xOz_Plane, name=f"{name}_mesh")

        boat2 = cpt.FloatingBody(mesh=mesh)
        boat2.add_all_rigid_body_dofs()
        boat2.keep_immersed_part()
        # boat2.show()

        dataset = make_database(body=boat2, omegas=omega, wave_directions=directions)

        from capytaine.io.xarray import separate_complex_values
        sep = separate_complex_values(dataset)
        sep.to_netcdf(file_out_nc,
                      encoding={'radiating_dof': {'dtype': 'U'},
                                'influenced_dof': {'dtype': 'U'},
                                'diffraction_result': {'dtype': 'U'}})

        print(f'saved NC results as {file_out_nc}')

        from mafredo import Hyddb1
        import numpy as np


        hyd = Hyddb1.create_from_capytaine(filename=file_out_nc)


        hyd.save_as(outfile)
        print(f'Saved as: {outfile}')

        # hyd.plot(do_show=True)



```python
