+++
title = "Analytical Reconstruction of a Simple Tree Amplitude"
subtitle = "Analytical reconstruction of a six-gluon NMHV tree amplitude"

# Add a summary to display on homepage (optional).
summary = "https://arxiv.org/abs/1904.04067"

date = 2019-05-21T19:16:01+01:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Giuseppe De Laurentis"]

# Is this a featured post? (true/false)
featured = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = []

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references 
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

# A Simple Tree Example


```python
# Initialisations
...
from antares.core.settings import settings
from antares.core.unknown import Unknown, BHUnknown
from antares.particles.particles import Particles
```


```python
oTree = BHUnknown(helconf="pmpmpm", amppart="tree")
oUnknown = Unknown(oTree, load_partial_results=False, silent=True)
```

Scalings


```python
oUnknown.do_single_collinear_limits()
```

    Finished calculating single scalings. The partial result is:
    /⟨1|2⟩[1|2]⟨1|6⟩[1|6]⟨2|3⟩[2|3]⟨3|4⟩[3|4]⟨4|5⟩[4|5]⟨5|6⟩[5|6]s_123s_234s_345
    
    The mass dimension of the unknown is -2.
    The phase weights of the unknown are [-2, 2, -2, 2, -2, 2].
    The mass dimension of the new unknown is 16.
    The phase weights of the new unknown are [-2, 2, -2, 2, -2, 2].


```python
oUnknown.do_double_collinear_limits()
```

    Finished calculating pair scalings. They are:
    [⟨1|2⟩, [1|2]]: 1.0, 2  → 2
    [⟨1|2⟩, ⟨1|6⟩]: 1.0, 30 → 5
    [⟨1|2⟩, [1|6]]: 1.0, 3  → 2
    ...
    [s_123, s_234]: 1.0, 2  → 2
    [s_123, s_345]: 1.0, 2  → 2
    [s_234, s_345]: 1.0, 2  → 2
    


Partial Fractioning


```python
invariants = oUnknown.poles_to_be_eliminated
print("Poles to be  eliminated:")
pprint(invariants)
print("")
lTerms = oUnknown.get_partial_fractioned_terms(invariants[0])
```

    Poles to be  eliminated:
    [s_123, s_234, s_345, ⟨1|2⟩, [1|2], ⟨1|6⟩, [1|6], ⟨2|3⟩, [2|3], ⟨3|4⟩, [3|4], ⟨4|5⟩, [4|5], ⟨5|6⟩, [5|6]]
    
    Forced:
    []
    Forbidden: 
    [⟨1|6⟩, [1|6], ⟨3|4⟩, [3|4], s_234, s_345]
    Optional: 
    [(⟨5|6⟩, ⟨1|(2+3)|4]), ([4|5], ⟨6|(1+2)|3]), (⟨4|5⟩, ⟨3|(1+2)|6]), (⟨1|2⟩, ⟨6|(1+2)|3]), ([2|3], ⟨1|(2+3)|4]), (⟨2|3⟩, ⟨4|(2+3)|1]), ([5|6], ⟨4|(2+3)|1]), ([1|2], ⟨3|(1+2)|6])]
    
    There are 11 symmetries:                 
    [(165432, False), (216543, True), (234561, True), (321654, False), (345612, False), (432165, True), (456123, True), (543216, False), (561234, False), (612345, True), (654321, True)]
    
    /⟨1|2⟩⟨2|3⟩[4|5][5|6]⟨1|(2+3)|4]⟨3|(1+2)|6]s_123                                                                                                                 
    
    /[1|2][2|3]⟨4|5⟩⟨5|6⟩⟨4|(2+3)|1]⟨6|(1+2)|3]s_123
    
    ...
    
    /⟨1|2⟩[2|3]⟨4|5⟩[5|6]⟨1|(2+3)|4]⟨3|(1+2)|6]⟨4|(2+3)|1]⟨6|(1+2)|3]s_123
    
    /[1|2]⟨2|3⟩[4|5]⟨5|6⟩⟨1|(2+3)|4]⟨3|(1+2)|6]⟨4|(2+3)|1]⟨6|(1+2)|3]s_123
    
    Found at least a result.
                                                                                      


Numerator Fitting


```python
oTerms = lTerms[0]
oTerms.fit_numerators()
```

    Starting fit of numerator coefficients for following ansatz:              
    /⟨1|2⟩⟨2|3⟩[4|5][5|6]⟨1|(2+3)|4]⟨3|(1+2)|6]s_123
    
    Inversion will proceed in s_123^1.0 collinear limit. No symmetry will be used.
    The following symmetries will then be appended to the result [(u'165432', False), (u'216543', True)]. Accuracy set to: -24
    Obtained ansatz from Daniel's spinor solve with lM, lPW: [8], [[0, 4, 0, 0, -4, 0]]. Size: 15.
    Finished loading augmented column.                                                                            Created new matrix of size 15x16
    Time elapsed loading the matrix: 0:00:00. Created new matrix of size 15x16.                                       
    Time elapsed in row reduction: 0:00:00. 0 elements of the ansatz were redundant!
    Nbr dropped redundant: 0, Nbr dropped zero: 10, Nbr dropped total: 10.
    
    Coeff. of ⟨1|2⟩⟨1|2⟩⟨1|2⟩⟨1|2⟩[1|5][1|5][1|5][1|5]: 1*I
    Coeff. of ⟨1|2⟩⟨1|2⟩⟨1|2⟩⟨2|3⟩[1|5][1|5][1|5][3|5]: -4*I
    Coeff. of ⟨1|2⟩⟨1|2⟩⟨2|3⟩⟨2|3⟩[1|5][1|5][3|5][3|5]: 6*I
    Coeff. of ⟨1|2⟩⟨2|3⟩⟨2|3⟩⟨2|3⟩[1|5][3|5][3|5][3|5]: -4*I
    Coeff. of ⟨2|3⟩⟨2|3⟩⟨2|3⟩⟨2|3⟩[3|5][3|5][3|5][3|5]: 1*I
    This piece correctly removes the singularity ([0])                                                 
    
    Refining the fit...
    Finished calculating single scalings. The partial result is:                                                              
    ⟨2|(1+3)|5]⁴/⟨1|2⟩⟨2|3⟩[4|5][5|6]⟨1|(2+3)|4]⟨3|(1+2)|6]s_123
    
    The mass dimension of the unknown is -2.
    The phase weights of the unknown are [-2, 2, -2, 2, -2, 2].
    The mass dimension of the new unknown is 0.
    The phase weights of the new unknown are [0, 0, 0, 0, 0, 0].
    
    Starting fit of numerator coefficients for following ansatz:              
    ⟨2|(1+3)|5]⁴/⟨1|2⟩⟨2|3⟩[4|5][5|6]⟨1|(2+3)|4]⟨3|(1+2)|6]s_123
    
    Inversion will proceed in s_123^1.0 collinear limit. No symmetry will be used.
    The following symmetries will then be appended to the result [(u'165432', False), (u'216543', True)]. Accuracy set to: -24
    Obtained ansatz from Daniel's spinor solve with lM, lPW: [0], [[0, 0, 0, 0, 0, 0]]. Size: 1.
    Finished loading augmented column.                                                                            Created new matrix of size 1x2
    Time elapsed loading the matrix: 0:00:00. Created new matrix of size 1x2.                                       
    Time elapsed in row reduction: 0:00:00. 0 elements of the ansatz were redundant!
    Nbr dropped redundant: 0, Nbr dropped zero: 0, Nbr dropped total: 0.
    
    Coeff. of this whole term is: 1*I
    This piece correctly removes the singularity ([0])                                                 



```python
print(oTerms)
```

    +1I⟨2|(1+3)|5]⁴/⟨1|2⟩⟨2|3⟩[4|5][5|6]⟨1|(2+3)|4]⟨3|(1+2)|6]s_123
    (u'165432', False)
    (u'216543', True)
    



```python
oParticles = Particles(6)
oParticles.fix_mom_cons()
assert(abs(oTerms(oParticles) - oTree(oParticles)) < 10 ** -280)
```
