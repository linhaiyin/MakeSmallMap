#游戏中小地图的制作
*大地图对象<br>
*大地图主角对象<br>
*大地图的宽度<br>
*大地图的高度<br>
*地图边界的数值检测<br>
*小地图主角的位置<br>
*GUI按钮是否被按下<br>
*小地图的背景贴图<br>
*小地图的主角贴图<br>
<hr>
*得到大地图的对象<br>
*的到大地图的主角对象<br>
*得到大地图默认宽度<br>
*得到大地图宽度的缩放比列<br>
*得到大地图的默认高度<br>
*得到大地图高度的缩放比列<br>
*原始宽度乘以缩放比列计算出真实宽/高度<br>
*越界监测的宽/高度<br>
<hr>
*绘制按键<br>
*绘制小地图背景<br>
*绘制小地图上的主角<br>
<hr>
*按键移动<br>
<hr>
###越界检测
*得到当前主角在地图中的坐标<br>
*当控制主角超过地图范围时，重新计算主角坐标 <br>
*根据比列计算小地图主角的坐标<br>
####CODE
<pre>public class smallMap : MonoBehaviour {
//大地图地形对象
GameObject plane;
//大地图主角对象
GameObject cube;

//大地图的宽度
float mapWidth;
//大地图的高度
float mapHeight;
//地图边界的检测数值
float widthCheck;
float heightCheck;

//小地图主角的位置
float mapcube_x = 0;
float mapcube_y = 0;


//GUI按钮是否被按下
bool keyUp;
bool keyDown;
bool keyLeft;
bool keyRight;

//小地图的背景贴图
public Texture map;
//小地图的主角贴图
public Texture map_cube;

void Start()
{
    //得到大地图对象
    plane = GameObject.Find("Plane");
    //得到大地图主角对象
    cube = GameObject.Find("Cube");
    //得到大地图默认宽度
    float size_x = plane.GetComponent<MeshFilter>().mesh.bounds.size.x;
    //得到大地图宽度的缩放比例
    float scal_x = plane.transform.localScale.x;
    //得到大地图默认高度
    float size_z = plane.GetComponent<MeshFilter>().mesh.bounds.size.z;
    //得到大地图高度缩放地理
    float scal_z = plane.transform.localScale.z;

    //原始宽度乘以缩放比例计算出真实宽度
    mapWidth = size_x * scal_x;
    mapHeight = size_z * scal_z;

    //越界监测的宽度
    widthCheck = mapWidth / 2;
    heightCheck = mapHeight / 2;

    check();
}

void OnGUI()
{
    keyUp = GUILayout.RepeatButton("向前移动");

    keyDown = GUILayout.RepeatButton("向后移动");

    keyLeft = GUILayout.RepeatButton("向左移动");

    keyRight = GUILayout.RepeatButton("向右移动");

    //绘制小地图背景
    GUI.DrawTexture(new Rect(Screen.width - map.width, 0, map.width, map.height), map);
    //绘制小地图上的“主角”
    GUI.DrawTexture(new Rect(mapcube_x, mapcube_y, map_cube.width, map_cube.height), map_cube);
}

void FixedUpdate()
{


    if (keyUp)
    {
        //向前移动
        cube.transform.Translate(Vector3.forward * Time.deltaTime * 5);
        check();

    }

    if (keyDown)
    {
        //向后移动
        cube.transform.Translate(-Vector3.forward * Time.deltaTime * 5);
        check();
    }

    if (keyLeft)
    {
        //向左移动
        cube.transform.Translate(-Vector3.right * Time.deltaTime * 5);
        check();
    }

    if (keyRight)
    {
        //向右移动
        cube.transform.Translate(Vector3.right * Time.deltaTime * 5);
        check();
    }




}

//越界检测
void check()
{
    //得到当前主角在地图中的坐标
    float x = cube.transform.position.x;
    float z = cube.transform.position.z;

    //当控制主角超过地图范围时，重新计算主角坐标
    if (x >= widthCheck)
    {
        x = widthCheck;
    }
    if (x <= -widthCheck)
    {
        x = -widthCheck;
    }
    if (z >= heightCheck)
    {
        z = heightCheck;
    }
    if (z <= -heightCheck)
    {
        z = -heightCheck;
    }

    cube.transform.position = new Vector3(x, cube.transform.position.y, z);

    //根据比例计算小地图“主角”的坐标
    //GUI 坐标是左上角为起点和屏幕坐标以左下角为起点
    mapcube_x = (map.width / mapWidth * x) + ((map.width / 2) - (map_cube.width / 2)) + (Screen.width - map.width);
     mapcube_y = map.height - ((map.height / mapHeight * z) + (map.height / 2));
  //  mapcube_y =((map.height / mapHeight * z) + (map.height / 2));
}
}




