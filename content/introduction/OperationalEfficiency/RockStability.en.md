---
title : "Rock stability analysis using GenAI"
weight : 23
---



#Heading

Python code for predicting Rock Stability locations using machine learning techniques.

1. Select Amazon bedrock


:image[synthetic image generation]{src="/static/IndustryPrompts/Selectbedrock.png" }

2. Open side bar

:image[synthetic image generation]{src="/static/IndustryPrompts/selectsidepanel.png" }

3. Select text

:image[synthetic image generation]{src="/static/IndustryPrompts/selecttext.png"}

4. Select the model Meta Llama 3.1 70B model

:image[synthetic image generation]{src="/static/IndustryPrompts/selectllama80b.png" width=800}


# Explain Use Case
Rock stability analysis is a complex task that involves various methods and techniques. Here, I'll provide you with a basic code for a simple rock stability analysis using the Rocscience software's RS2 program, which is a widely used tool for rock mechanics and geotechnical engineering. Please note that this is a simplified example and actual rock stability analysis requires more detailed input data and consideration of various factors.


**Please note that this is a highly simplified example and actual rock stability analysis in mining requires more detailed input data and consideration of various factors, such as joint sets, faults, and other geological features.**





:::code{showCopyAction=true showLineNumbers=false language=python}
import numpy as np
from scipy.sparse import csr_matrix
from scipy.sparse.linalg import spsolve

# Define material properties
E = 30e9  # Young's modulus (Pa)
nu = 0.25  # Poisson's ratio

# Define geometry and mesh
nodes = np.array([[0, 0], [1, 0], [1, 1], [0, 1]])
elements = np.array([[0, 1, 2], [2, 3, 0]])
n_nodes = len(nodes)
n_elements = len(elements)

# Define boundary conditions
fixed_nodes = [0, 1]
force_nodes = [2]
force_values = [1e6]  # 1 MN force

# Calculate element stiffness matrices
def calc_element_stiffness(E, nu, nodes, element):
    x1, y1 = nodes[element[0]]
    x2, y2 = nodes[element[1]]
    x3, y3 = nodes[element[2]]

    k = np.array([
        [1, x1, y1, 1, x2, y2, 1, x3, y3],
        [x1, x1**2, x1*y1, x2, x2**2, x2*y2, x3, x3**2, x3*y3],
        [y1, x1*y1, y1**2, y2, x2*y2, y2**2, y3, x3*y3, y3**2],
        [1, x2, y2, 1, x3, y3, 1, x1, y1],
        [x2, x2**2, x2*y2, x3, x3**2, x3*y3, x1, x1**2, x1*y1],
        [y2, x2*y2, y2**2, y3, x3*y3, y3**2, y1, x1*y1, y1**2],
        [1, x3, y3, 1, x1, y1, 1, x2, y2],
        [x3, x3**2, x3*y3, x1, x1**2, x1*y1, x2, x2**2, x2*y2],
        [y3, x3*y3, y3**2, y1, x1*y1, y1**2, y2, x2*y2, y2**2]
    ])

    B = 1 / (2 * E / (1 - nu**2)) * np.array([
        [1, nu, 0],
        [nu, 1, 0],
        [0, 0, (1 - nu) / 2]
    ])

    k_element = B.T @ k @ B * np.linalg.det([
        [x2 - x1, x3 - x1],
        [y2 - y1, y3 - y1]
    ])

    return k_element

# Assemble global stiffness matrix
K = csr_matrix((n_nodes * 2, n_nodes * 2))
for element in elements:
    k_element = calc_element_stiffness(E, nu, nodes, element)
    for i in range(3):
        for j in range(3):
            i_global = element[i] * 2
            j_global = element[j] * 2
            K[i_global:i_global + 2, j_global:j_global + 2] += k_element[i * 2:i * 2 + 2, j * 2:j * 2 + 2]

# Apply boundary conditions
fixed_dofs = np.concatenate([2 * np.array(fixed_nodes), 2 * np.array(fixed_nodes) + 1])
free_dofs = np.setdiff1d(np.arange(K.shape[0]), fixed_dofs)
K_free = K[free_dofs, :][:, free_dofs]

# Apply forces
force_dofs = np.concatenate([2 * np.array(force_nodes), 2 * np.array(force_nodes) + 1])
f_free = np.zeros(K_free.shape[0])
for dof, force in zip(force_dofs, force_values):
    f_free[np.where(free_dofs == dof)[0]] = force

# Solve for displacements
u_free = spsolve(K_free, f_free)
u = np.zeros(K.shape[0])
u[free_dofs] = u_free

# Calculate stresses
stresses = []
for element in elements:
    x1, y1 = nodes[element[0]]
    x2, y2 = nodes[element[1]]
    x3, y3 = nodes[element[2]]

    B = np.array([
        [1, x1, y1, 1, x2, y2, 1, x3, y3],
        [0, 1, 0, 0, 1, 0, 0, 1, 0],
        [0, 0, 1, 0, 0, 1, 0, 0, 1]
    ])

    u_element = np.array([
        u[element[0] * 2], u[element[0] * 2 + 1],
        u[element[1] * 2], u[element[1] * 2 + 1],
        u[element[2] * 2], u[element[2] * 2 + 1]
    ])

    D = E / (1 - nu**2) * np.array([
        [1, nu, 0],
        [nu, 1, 0],
        [0, 0, (1 - nu) / 2]
    ])

    stress = D @ B @ u_element

    stresses.append(stress)

# Print results
for i, stress in enumerate(stresses):
    print(f"Element {i}: Stress = {stress}")

:::



# Explain Use Case

TIn this synthetic data:

- The geometry consists of a rectangular domain with dimensions 3 x 2 units.
- The mesh is a structured grid of 3 x 2 quadrilateral elements.
- The material properties are defined as `E = 30 GPa` and `nu = 0.25`.
- The boundary conditions fix the nodes at the four corners of the domain.
- External forces of 100 kN are applied at the middle nodes on the top and bottom edges.

You can directly substitute these values into the provided Python code to test its functionality. Additionally, you can modify these synthetic data to simulate different scenarios, such as irregular geometries, varying material properties, or different boundary conditions and loading cases.

Note that this synthetic data is a simplified example, and in real-world applications, you would typically obtain the input data from field measurements, geological surveys, or other sources specific to the mining site under investigation.

:::code{showCopyAction=true showLineNumbers=false language=bash}
Given me code to generate synthetic data that will be needed for that csv file 
:::




:::code{showCopyAction=true showLineNumbers=false language=python}
import numpy as np

# Define material properties
E = 30e9  # Young's modulus (Pa)
nu = 0.25  # Poisson's ratio

# Define geometry and mesh
nodes = np.array([
    [0, 0], [1, 0], [2, 0], [3, 0],  # Bottom nodes
    [0, 1], [1, 1], [2, 1], [3, 1],  # Middle nodes
    [0, 2], [1, 2], [2, 2], [3, 2]   # Top nodes
])
elements = np.array([
    [0, 1, 4, 5], [1, 2, 5, 6], [2, 3, 6, 7],  # Bottom elements
    [4, 5, 8, 9], [5, 6, 9, 10], [6, 7, 10, 11]  # Top elements
])
n_nodes = len(nodes)
n_elements = len(elements)

# Define boundary conditions
fixed_nodes = [0, 3, 8, 11]  # Fixed nodes at the corners
force_nodes = [1, 2, 5, 6, 9, 10]  # Nodes with applied forces
force_values = [1e5, 1e5, 1e5, 1e5, 1e5, 1e5]  # 100 kN forces

# You can use these synthetic data values to test the provided code
:::

##install code in cloud9 

:::code{showCopyAction=true showLineNumbers=false language=bash}
pip install  ace_tools contourpy cycler fonttools joblib kiwisolver matplotlib numpy packaging pandas pillow pyparsing python-dateutil pytz scikit-learn scipy six threadpoolctl tzdata
:::

## Results 


:image[synthetic image generation]{src="/static/operations/result1.png" }
