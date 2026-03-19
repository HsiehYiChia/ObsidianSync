---

---
Q: You have a collection of points on a 2-dimensional plane. Please find the bounding box of the collection of points.

- Input: (0, -1), (4, 2), (-2, 0), (1, -4), (3, 1)
- Output: (-2, -4) (4, 2)
- Add( 3,3)
- Remove( 3,1)

Struct point {

Int x;

Int y;

}

vector<point> points;

vector<point> get_bounding_box(vector<point> points)

{

Int left=INT_MAX, right=INT_MIN, up=INT_MAX, bottom=INT_MIN;

For (int = i = 0; i < points.size(); i++) {

If (points[i].x < left)

Left = points[i].x;

If (points[i].x > right)

right= points[i].x;

If (points[i].y < up)

up= points[i].y;

If (points[i].y > bottom)

bottom= points[i].y;

}

Point tl, br;

tl.x= left;

tl.y = up;

Br.x = right;

Br.y = bottom;

vector<points> res;

res.push_back(tl);

res.push_back(br);

Return res;

}

Q2:  implement add/ remove function

- Add( 3,3)
- Remove( 3,1)

priority_queue<point, mini_x> max_left;

priority_queue<point, greater_x> max_right;

priority_queue<point, mini_y> max_top;

priority_queue<point, greater_y> max_bottom;

vector<point> get_res()

{

Point tl, br;

Tl.x = max_left.top();

Tl.y = max_top.top();

Br.x = max_right.top();

Br.y = max_bottom.top();

vector<point> res;

res.push_back(tl);

res.push_back(br);

Return res;

}

vector<point> get_bounding_box(vector<point> points)

{

For (int i = 0; i < points.size();i++) {

max_left.push(points[i]);

max_right.push(points[i]);

max_top.push(points[i]);

max_bottom.push(points[i]);

}

Return get_res();

}

vector<point> add(point p)

{

max_left.push(p);

max_right.push(p);

max_top.push(p);

max_bottom.push(p);

Return get_res();

}

vector<point> remove(point p)

{

max_left.erase(p);

max_right.erase(p)

max_top.erase(p)

max_bottom.erase(p)

Return get_res();

}

Q3:  Put N points in to bounding box  to check if it’s in bounding box

Bool validate(vector<point> points)

{

For (int i = 0; i < points.size(); i++) {

If (points[i].x < left ||

points[i].x > right ||

Points[i].y < top ||

Points[i].y > bottom)

Return false;

}

Return true;

}