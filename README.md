# Count-Number-of-Trapezoids-II

You are given a 2D integer array points where points[i] = [xi, yi] represents the coordinates of the ith point on the Cartesian plane.

Return the number of unique trapezoids that can be formed by choosing any four distinct points from points.

A trapezoid is a convex quadrilateral with at least one pair of parallel sides. Two lines are parallel if and only if they have the same slope.

class Solution:
    def countTrapezoids(self, A: List[List[int]]) -> int:
        slopes = Counter()
        lines = Counter()
        mids = Counter()
        midlines = Counter()

        for (x1, y1), (x2, y2) in combinations(A, 2):
            dx, dy = x2 - x1, y2 - y1
            g = gcd(dx, dy)
            dx, dy = dx // g, dy // g
            if dx < 0 or (dx == 0 and dy < 0):
                dx, dy = -dx, -dy

            inter = dx * y1 - dy * x1
            slopes[dx, dy] += 1
            lines[dx, dy, inter] += 1
            mids[x1 + x2, y1 + y2] += 1
            midlines[x1 + x2, y1 + y2, dx, dy, inter] += 1

        ans = sum(comb(v, 2) for v in slopes.values())
        ans -= sum(comb(v, 2) for v in lines.values())
        ans -= sum(comb(v, 2) for v in mids.values())
        ans += sum(comb(v, 2) for v in midlines.values())
        return ans
