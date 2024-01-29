+++
title = 'Lab 6: Selection technique Implementation'
date = 2024-01-20T10:35:20+01:00
draft = false
summary = "Implementaion of my selection techniques in a supermarket."
tags = ["Lab"]

+++
> TODO:
>
> Add locomotion
>
> 
In this lab, I am going to talk about the implmentation of my selection thechnique. The project is build based on a provided project. In this project, it gives the example code of raycasting technique. And I will gradually inform my design process.
## Locomotion
The first step was to add movement functionality to the project since user movement is required during selection tasks.

Find the **OVRPlayerController** prefab and drag it to our scene. The player character uses a capsule as the collider, so change the scale properly. The **OVRCamera** should be a child of **OVRPlayerController** so we move the player and we can have a first person mobile experience. 
{{< figure src="player.png" height=300 align=center  >}}

We can walk around the scene using the left controller joystick and turn around using the right joystick now.

## Cursor
As I introduced in the previous blog, my technique is an incremental improvement on that technique. To make the raycasting selection more flexible, I will add a controllable cursor on the ray. Instead of using the raycast hit point as the selection target, use this cursor. By holding the right HandTrigger and push right Joystick, users can move the cursor on the ray. 

Many codes are required for the properties of the cursor. By default, the ray with cursor should be similar as the original raycasting. Only if users are holding right HandTrigger, the cursor can be moved.
```
//sample of changing cursor properties if the ray hit an object
//if hit, move the cursor to the hit point, else move it to a defalut position.
if (hasHit)
            {
                cursorDistance += 2 * cursormove * Time.deltaTime;
                Vector3 newCursorPos = rightControllerTransform.position + rightControllerTransform.forward * cursorDistance;

                if (cursorInstance != null)
                {
                    if (cursormove == 0f && !handtrigger) {
                        cursorInstance.transform.position = hit.point;
                    }
                    else { cursorInstance.transform.position = newCursorPos;
                        handtrigger = true;
                    }
                    cursorInstance.SetActive(true);
                }
            }
```

The cursor should handle collision with the supermarket objects, so I add a **CursorCollisionDetector.cs** to the cursor. It can highlight the collision object.
Here is the example code:
```
    private void OnTriggerEnter(Collider other)
    {
        Debug.Log("Cursor entered the collider of: " + other.gameObject.name);
        gameObject.GetComponent<Outline>().enabled = true;
        currenthit = other.gameObject;

    }

    private void OnTriggerExit(Collider other)
    {
        Debug.Log("Cursor exited the collider of: " + other.gameObject.name);
        gameObject.GetComponent<Outline>().enabled = false;
        currenthit = null;
    }
```
{{< figure src="cursor.png" height=300 align=center  >}}
## Depth
The original idea for the depth controll is to use a flashlight. The flashlight should be a light source emitted from the user's controller. The base of the cone should be the depth surface. The tip of the cone is where the controller is. However, I got many issues when implemented this. For example, this cone shape is not suitable for handling the collisons.
{{< figure src="cube.png" height=300 align=center  >}}

By adding a **CollisionDetection.cs** script, I can handle the collisions. If I want to select an object behind an object, this function allows the user to see it by making the object in front of the depth surface transparent.

```
//part of the CollisionDetection code
private void Update()
    {
        tasktarget = Task.GetComponent<TaskManager>().GetCurrentObjectToSelect();
        Debug.Log("get current target" + tasktarget.GetInstanceID());
        Collider[] colliders = Physics.OverlapBox(myCollider.bounds.center, myCollider.bounds.extents, Quaternion.identity);
        HashSet<Collider> currentColliders = new HashSet<Collider>(colliders);

        foreach (var collider in currentColliders)
        {
            if (collider.gameObject.layer != ignoreLayer && collider.gameObject.layer != playerLayer && collider.gameObject.layer != groundLayer)
            {
                OnTriggerEnterManually(collider);
            }
            else
            {
                Debug.Log("something get"+collider.gameObject.layer);
            }
        }

        foreach (var collider in collidersInside)
        {
            if (!currentColliders.Contains(collider))
            {
                if (collider.gameObject.layer != ignoreLayer && collider.gameObject.layer != playerLayer && collider.gameObject.layer != groundLayer)
                {
                    OnTriggerExitManually(collider);

                }

            }
        }

        collidersInside = currentColliders;

    }
```
So I change my idea and use a cube to get the cursor depth. The objects that collide with blocks will become translucent. Here is how it looks like.
{{< figure src="changedepth.gif" height=300 align=center  >}}

## Result
For this project, I mainly add two funtion to the original raycasting: **Cursor** and **Depth**. My technique propse enhancements to the original Raycasting technique. It reduces error rate in occluded conditions such as selecting objects in supermarket.
[![Watch the video](video.png)](https://drive.google.com/file/d/1NNzwa27u3ifCsXvlxcf4x0l0CrGQZACJ/view?usp=sharing)
[Find on Github](https://github.com/GAO567/igd301final.git)







