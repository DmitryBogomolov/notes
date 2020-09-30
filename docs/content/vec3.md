# (x, y, z) vector with some frequently used operations

```python
from typing import NamedTuple
import math
import unittest

def _eq(a: float, b: float) -> bool:
    return math.fabs(a - b) < 1E-14

class vec3(NamedTuple):
    x: float
    y: float
    z: float

    def __eq__(self, other):
        return _eq(self.x, other.x) and _eq(self.y, other.y) and _eq(self.z, other.z)

    def len(self) -> float:
        return math.sqrt(dot(self, self))

    def is_zero(self) -> bool:
        return _eq(self.len(), 0)

    def is_unit(self) -> bool:
        return _eq(self.len(), 1)

    def mul(self, k: float) -> 'vec3':
        return self.__class__(self.x * k, self.y * k, self.z * k)

    def neg(self) -> 'vec3':
        return self.mul(-1)

    def norm(self) -> 'vec3':
        return self.mul(1 / self.len())

    def project_axix(self, axis: 'vec3') -> 'vec3':
        n_axis = axis.norm()
        return n_axis.mul(dot(self, n_axis))

    def project_plane(self, normal: 'vec3') -> 'vec3':
        return sub(self, self.project_axix(normal))

    def rotate_axis(self, axis: 'vec3', angle: float) -> 'vec3':
        project = self.project_axix(axis)
        reject = sub(self, project)
        coreject = cross(axis.norm(), reject)
        rotated_reject = add(reject.mul(math.cos(angle)), coreject.mul(math.sin(angle)))
        return add(project, rotated_reject)


zero = vec3(0, 0, 0)
e_x = vec3(1, 0, 0)
e_y = vec3(0, 1, 0)
e_z = vec3(0, 0, 1)

def add(a: vec3, b: vec3) -> vec3:
    return vec3(a.x + b.x, a.y + b.y, a.z + b.z)

def sub(a: vec3, b: vec3) -> vec3:
    return vec3(a.x - b.x, a.y - b.y, a.z - b.z)

def dot(a: vec3, b: vec3) -> float:
    return a.x * b.x + a.y * b.y + a.z * b.z

def cross(a: vec3, b: vec3) -> vec3:
    return vec3(a.y * b.z - a.z * b.y, a.z * b.x - a.x * b.z, a.x * b.y - a.y * b.x)

def check_orthogonal(a: vec3, b: vec3) -> bool:
    return _eq(dot(a, b), 0)

def check_collinear(a: vec3, b: vec3) -> bool:
    return cross(a.norm(), b.norm()).is_zero()

def angle(a: vec3, b: vec3) -> float:
    return math.acos(dot(a.norm(), b.norm()))


# ====
class TestVec3(unittest.TestCase):
    def test_len(self):
        self.assertEqual(vec3(1, 2, 2).len(), 3)

    def test_is_zero(self):
        self.assertEqual(e_x.is_zero(), False)
        self.assertEqual(zero.is_zero(), True)
        self.assertEqual(vec3(1 / 3, 2 / 3, 2 / 3).is_zero(), False)

    def test_is_unit(self):
        self.assertEqual(e_x.is_unit(), True)
        self.assertEqual(zero.is_unit(), False)
        self.assertEqual(vec3(1 / 3, 2 / 3, 2 / 3).is_unit(), True)

    def test_norm(self):
        self.assertEqual(vec3(1, 2, 2).norm(), vec3(1 / 3, 2 / 3, 2 / 3))
        self.assertEqual(vec3(1 / 3, 2 / 3, 2 / 3).norm(), vec3(1 / 3, 2 / 3, 2 / 3))

    def test_mul(self):
        self.assertEqual(vec3(1, 2, 3).mul(2), vec3(2, 4, 6))

    def test_neg(self):
        self.assertEqual(vec3(1, 2, 3).neg(), vec3(-1, -2, -3))
        self.assertEqual(vec3(1, 2, 3).neg().neg(), vec3(1, 2, 3))

    def test_add(self):
        self.assertEqual(add(vec3(1, 2, 3), vec3(3, 4, 5)), vec3(4, 6, 8))

    def test_sub(self):
        self.assertEqual(sub(vec3(1, 2, 3), vec3(3, 4, 5)), vec3(-2, -2, -2))

    def test_dot(self):
        self.assertEqual(dot(e_x, e_x), 1)
        self.assertEqual(dot(e_y, e_z), 0)
        self.assertEqual(dot(vec3(1, 2, 3), vec3(3, 4, 5)), 26)

    def test_cross(self):
        self.assertEqual(cross(e_x, e_x), zero)
        self.assertEqual(cross(e_x, e_y), e_z)
        self.assertEqual(cross(vec3(1, 2, 3), vec3(3, 4, 5)), vec3(-2, 4, -2))

    def test_project_axis(self):
        v = vec3(10, 20, 30)

        self.assertEqual(v.project_axix(e_x), vec3(10, 0, 0))
        self.assertEqual(v.project_axix(e_y), vec3(0, 20, 0))
        self.assertEqual(v.project_axix(e_z), vec3(0, 0, 30))

        self.assertEqual(v.project_axix(e_x.neg()), vec3(10, 0, 0))
        self.assertEqual(v.project_axix(e_y.neg()), vec3(0, 20, 0))
        self.assertEqual(v.project_axix(e_z.neg()), vec3(0, 0, 30))

        self.assertEqual(v.project_axix(vec3(1, 2, 0)), vec3(10, 20, 0))
        self.assertEqual(v.project_axix(vec3(0, 2, 3)), vec3(0, 20, 30))
        self.assertEqual(v.project_axix(vec3(1, 0, 3)), vec3(10, 0, 30))
        self.assertEqual(v.project_axix(vec3(1, 2, 3)), v)
        self.assertEqual(v.project_axix(v), v)

    def test_check_orthogonal(self):
        self.assertEqual(check_orthogonal(e_x, e_y), True)
        self.assertEqual(check_orthogonal(e_z, e_z), False)
        self.assertEqual(check_orthogonal(vec3(0, 3, 0), vec3(1, 0, 2)), True)
        self.assertEqual(check_orthogonal(vec3(1, 3, 0), vec3(1, 0, 2)), False)

    def test_check_collinear(self):
        self.assertEqual(check_collinear(e_x, e_x), True)
        self.assertEqual(check_collinear(e_y, e_z), False)
        self.assertEqual(check_collinear(vec3(1, 2, 3), vec3(2, 4, 6)), True)
        self.assertEqual(check_collinear(vec3(1, 2, 4), vec3(2, 4, 6)), False)
        self.assertEqual(check_collinear(vec3(1, 2, 3), vec3(1, 2, 3).neg()), True)

    def test_rotate_axis(self):
        self.assertEqual(vec3(1, 2, 3).rotate_axis(e_z, math.pi / 2), vec3(-2, 1, 3))
        self.assertEqual(vec3(1, 2, 3).rotate_axis(e_z, -math.pi / 2), vec3(2, -1, 3))
        self.assertEqual(vec3(1, 2, 3).rotate_axis(e_z, math.pi), vec3(-1, -2, 3))

    def test_angle(self):
        self.assertEqual(angle(vec3(1, 2, 3), vec3(2, 4, 6)), 0)
        self.assertEqual(angle(vec3(1, 2, 3), vec3(-1, -2, -3)), math.pi)
        self.assertEqual(angle(vec3(1, 2, 0), vec3(2, -1, 0)), math.pi / 2)


if __name__ == '__main__':
    unittest.main()
```
