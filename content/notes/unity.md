---
title: "Unity"
date: 2020-09-14T11:32:14+08:00
---

# Unity

## Shortcuts
选中物体 F 居中
V 顶点吸附
Command + D: Duplicate a GameObject
Command + Shift + F: quick set transform attribute to the selected object

## General Stuffs
Use parent-child relationship to combine several objects into one.
(Use a new empty object to contain the models)
Player: Empty object as Parent, children: player model, camera.
position of child object is relevant to the parent 
**Apply Material (Material is an instance of a shader)**
Create a material
GameObject - Inspector - Mesh Renderer - Materials - Element
Four Modes:
Opaque (The A value in RGBA will not take effect)
Cutout (only display the non-transparent part of the pic)
Fade (Change the A value in RGBA, the object can be completely invisible)
Transparent (Change the A value in RGBA, always visible, e.g. glass)


## Camera
**Clear Flags:** One attribute of Camera, can be set as skybox/solid colour/etc. (Are the areas without GameObject)
**Skybox:** the material used to simulate sky, wrap the whole scene.
Can be added through the component of camera or windows-lighting-skybox (recommended):
Can reflect the colour of the sky to GameObjects
- 6 sided (use six pics to wrap the scene)
	- Procedural (change the attribute of sun, sky and ground)
		- Cube-map(rarely used)
**Culling Mash:** 选择遮罩 about the layer attribute
Select which layers are displayed in the camera view
**Projection:** Perspective(3D)/Orthographic(2D)
**Field of View(FOV)** 
**Clipping Planes:** How far and how near the GameObject will be rendered
**Viewport Rect:** scale camera view and place it on a specific place on the screen.
e.g. mini map, split the screen for two players
- Depth: set `mini map > main camera` to always display min map.

Mini map use orthographic projection and set depth, use a plane(e.g. a triangle) to represent the player one the screen, use layer to control display (above the inspector menu)
Use a big 2D plane of the scene for the camera instead of the camera directly look at the scene (save rendering resource)
Set clear flag as *depth only*

Instant Occlusion Culling:
删除摄像机视角内看不到的物体 objects多且密集时应用 (判定遮挡需要消耗CPU资源)


## Unity Script
*Tamplate*
```cs
using UnityEngine;
using System.Collections;

public class Demo01 : MonoBehaviour
{
	// The two function can be deleted if not needed
	// Start is called before the first frame start
	void Start()
	{
	}
	
	// Update is called per frame
	void Update()
	{
	}
}
```

*Show and hide variable in inspector*
```cs
// Show private variable in inspector
[SerializeField]
private int a = 30;

// Hide the public variable in inspector
[hideInInspector]
public int b = 20;

// show a slide bar in inspector
[Range(0-100)]
public int c;

```

Cannot access parent thread in child thread.
Do not use unity function in constructor in script.


## Script Lifecycle
### Initialisation
```cs
// Awake is called after as soon as GameObject is constucted
// Will be called when the script is enabled
private void Awake()
{
	Debug.Log("Awake--" + Time.time + "--" + this.name)
}

// Start() is called after all Awake() is called, the script is enable
void Start()
```

In `Awake()`
Use to figure whether to enable this script or not.
`this.enable = true`

### Physics Phase
```cs
// FixedUpdate() is called at a fixed time interval (default 0.02s)
// Used to update the physical effect to a GameObject (move, rotate..)
// Will not be affected by rendering
private void FixedUpdate()

// Update() is called per frame, not a fixed interval
// Game Logic is implemented in Update
// For consistency, moving is implemented in Update (will be affected by frame randering)
private void Update()

// LateUpdate() is called after Update() in one frame
// Following logic can be implement here
private void LateUpdate()
```

### Input Event
```cs
// Is depending on collider
// When mouse is pressed on the collider
private void OnMouseDown()
private void OnMouseUP()

private void OnMouseEnter() // Move in the collider

private void OnMouseOver() // Pass

private void OnMouseExit() // Exit
```

### Screen Rendering
```cs
// Is called when the Mesh Renderer is visble on any camera
private void OnBecameVisible()

// Called when Mesh Renderer is invisible to all camera
private void OnBecameInvisible()
```

### Finishing
```cs
private void OnDisable() // The scrpit is disabled
private void OnDestroy()
private void OnApplicationQuit()
```


## Debugging
1. Console `Debug.Log()`, `print()` 
	`print()` is a function of mono-behaviour 
	*Delete after use, debug consume significant computing power*

2. Define a public variable, check the value in inspector

3. *vstu* debugging tool
	Set break point
	Enter debugging mode
	Play in Unity

## Component
The game object this component is attached, a component is always attached to a game object
```cs
// transform.position is a Vector3
this.transform.position = new Vector3(0,0,10);
// Set color
this.GetComponent<MeshRenderer>().material.color = Color.blue;
```

### Transform
The transform of child object is local position (relative to the parent)
All transform is local position
```cs
foreach (Transform child in transform)
{
	print(child.name)
}
```

**Position, Rotation and Scale**
```cs
// The position relative to the world axis
this.transform.position

// The position relative to the center of the parent object
this.transform.localPosition

// Relative scale to the parent object
this.transform.localScale

// Scale relative to the model
this.transform.lossyScale //Read only
// e.g. parent localScale of parent = 3, localScale = 2 lossyScale = 6

// Move 1 meter along the z axis of the object itslef
this.transform.Translate(0,0,1)
// Relative
Translate(Vector3 translation, Transform relativeTo)

// Move along World axis
this.transform.Translate(0, 0, 1, Space.World)

// Rotate
this.transform.Rotate(0,0,1)
this.transform.Rotate(0, 0, 1, Space.World)
// Rotate to face straight to 
this.transform.LookAt()

// Overload 
this.transform.RotateAround(Vector3 point, Vector3 axis, float degree)
```

**Find/Set parent, child, root**
```cs
// The root object component (the final parent)
Transform rootTF = this.transform.root;

// Get parent component
Transform parentTf = this.transfrom.parent; //Read Only

// Set parent
public Transform tf;
this.transform.SetParent(Transform parent, bool worldPositionStays = true);
// if the worldPositionStays is set to false, the current transform is treated as relative position to the parent
// true -> transform is treated as world position

//Find child by name
this.transform.Find(string name)
name = "LeftShoulder/Arm/Head"
// the name of the child, return transform component, cannot find grandchild

//Find child by index
int count = this.transform.childCount;
for (int i = 0l i<count; i++)
{
	Transform childTF = this.transform.GetChild(i)
}

// Use recursion to find a certian object if layer not known
// Seperate file of a tool, does not inherit MonoBehaviour
public class TransformHelper
{
	public static Transform GetChild(Trasnform parentTF, string childName)
	{
		Transform childTF = parentTf.Find(childName)
		if(childTF != null)
			return childTF;
		// find child in child
		for (int i = 0; i < count; i++)
		{
			childTF = GetChild(parentTF.GetChild(i),childName);
			if(childTF != null)
				return childTF;
		}
		return null;
}
```

### GameObject
`activeInHierarchy`: Is the GameObject active in the scene?
`activeSelf`: The local active state of this GameObject (Read Only)
`SetActive`: Activates/Deactivates the GameObject

`this.gameObject.activeHierarchy();`

```cs
// Create a GameObject
GameObject light = new GameObject();
// Create a component
Light light = this.gameObject.AddComponent<Light>();
light.color = Color.red;
light.type = LightType.Point;

// Find a GameObject in scene (consume a lot of computing power)
GameObject.Find(string object_name);
// Use these instead:
// Return all GameObjects with the tag
GameObject[] allEnemy = GameObject.FindGameObjectsWithTag(string tag_name);
// Return one GameObject with the tag
GameObject Player1 = GameObject.FindWithTag("Player");
```

Your code will never directly create a Component in script.
Use `gameObject.AddComponent<name>();`

### Object
```cs
// Removes a GameObject, component or asset
Object.Destroy(gameObject, int time_delay)

//DontDestroyOnLoad
void Awake()
{
	DontDestroyOnLoad(transform.gameObject);
}

//Clones the object original and returns the clone
Instantiate(Object original) // use defalut setting of the object
Instantiate(Object original, Vector3 position, Quaternion rotation);
```

**Fine Objects**
```cs
//Type is the class name of the script, e.g.:
class Enemy : MonoBehaviour
{}

// Return the first active loaded object of Type type
Object.FindObjectOfType<Type>();

//Returns a list of all active loaded objects of Type type
Object.FindObjectsOfType<Type>();
```



`Vector3.Distance(transform.position, transform.postion)`


**Finding GameObjects**
```cs
//By name
player = GameObject.Find("Hero");

// By Tag
// Find an object
player = GameObject.FindWithTag("hero");
// Find a collection of objects
enemies = GameObject.FindGameObjectsWithTag("Enemy");
```

Physics update: FixedUpdate()
Frame update: Update()

After the Update and FixUpdate: LateUpdate()

**Physics events**
```cs
OnCollisionEnter
OnCollisionStay
OnCollisionExit

OnTriggerEnter
OnTriggerStay
OnTriggerExit

void OnCollisionEnter(otherObj: Collision)
{
	if (otherObj.tag == "Arrow")
	{
		ApplyDamage(10);
	}
}

```

**Time and Frame rate Management**
```cs
public class ExampleScript : MonoBehaviour {
    public float distancePerFrame;
    
    void Update() {
        transform.Translate(0, 0, distancePerFrame);
    }
}

public class ExampleScript : MonoBehaviour {
    public float distancePerSecond;
    
    void Update() {
        transform.Translate(0, 0, distancePerSecond * Time.deltaTime);
    }
}
```


**Time Scale**
“Bullet-time”
```cs
public class ExampleScript : MonoBehaviour {
    void Pause() {
        Time.timeScale = 0;
    }
    
	void Slow() {
		Time.timeScale = 0.5f;
	}

    void Resume() {
        Time.timeScale = 1;
    }
}
```


**Destroy**
```cs
void OnCollisionEnter(Collision otherObj) {
    if (otherObj.gameObject.tag == "Missile") {
        Destroy(gameObject,.5f);
    }
}

Destroy(this);
```

**Coroutine**
Pause execution and return control to unity but then continue where it left off on the following frame.
```cs
IEnumerator Fade() 
{
    for (float ft = 1f; ft >= 0; ft -= 0.1f) 
    {
        Color c = renderer.material.color;
        c.a = ft;
        renderer.material.color = c;
        yield return null;
    }
}
```
The function should be declared with a return type of IEnumerator and with the yield return statement included somewhere in the body.

To set a coroutine running, you need to use the `StartCoroutine` function
```cs
void Update()
{
    if (Input.GetKeyDown("f")) 
    {
        StartCoroutine("Fade");
    }
}
```

A coroutine is resumed on the frame after it yields but it is also possible to introduce a time delay using `WaitForSeconds`
```cs
IEnumerator Fade() 
{
    for (float ft = 1f; ft >= 0; ft -= 0.1f) 
    {
        Color c = renderer.material.color;
        c.a = ft;
        renderer.material.color = c;
        yield return new WaitForSeconds(.1f);
    }
}
```

Use coroutine for optimisation: an alarm that warns the player if an enemy is nearby. 
```cs
function ProximityCheck() 
{
    for (int i = 0; i < enemies.Length; i++)
    {
        if (Vector3.Distance(transform.position, enemies[i].transform.position) < dangerDistance) {
                return true;
        }
    }
    
    return false;
}

IEnumerator DoCheck() 
{
    for(;;) 
    {
        ProximityCheck();
        yield return new WaitForSeconds(.1f);
    }
}
```

Stop a Coroutine
`StopCoroutine`
`StopAllCoroutines`