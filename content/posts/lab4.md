+++
title = ' Lab 4: Roll-a-ball in VR'
date = 2023-12-06T10:35:20+01:00
draft = false
summary = "Convert previous lab to VR version."

tags = ["Lab"]

+++

In the previous experiment, we created the **Roll a ball** game where we could control the ball with our keyboard. Now, we will convert it into a VR version. Let's first create a new 3D project and name it as **"Roll-a-ball-VR"**.
## Reuse Roll a ball
Before we start our new project, we export our previous work as a **.unitypackage** file. Then we open our new project and import this package.

We may see some warnings due to **PlayerController.cs** script name conflicts. Since we are going to use the input provided by Oculus Intergration instead of Input System. We need to remove the code from the input system and rename our PlayerController.cs.
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
//using UnityEngine.InputSystem;
using TMPro;
public class PlayerControllerRAB : MonoBehaviour
{
    ...
 // void OnMove(InputValue movementValue)
    // {
    //     Vector2 movementVector = movementValue.Get<Vector2>();
    //     movementX = movementVector.x;
    //     movementY = movementVector.y;
    // }
    ...
}
```
## Meta SDK
Find **Meta XR All-In-One SDK** on Unity asset store and import it to our project.
{{< figure src="meta.png" height=400 align=center  >}}
Go to **Edit > Project Settings > XR Plug-in Management** and check Oculus in the Android tab.
{{< figure src="xr.png" height=400 align=center  >}}
Then go to **Project Settings > Oculus** and fix all issues.
{{< figure src="fix.png" height=400 align=center  >}}

## Setup scene
First, we add a huge floor and a table for the Roll a ball game.

Insteading of using MainCamera, we find OVRCameraRig prefab in Assets and drag it to the scene. Click it and find OVRManager component in Inspector. Set the Tracking Origin Type to **Floor Level**.

Then find OVRControllerPrefab in Assets and drag it as a Child of LeftControllerAnchor. Select Controller as **L touch** in OVRControllerHelper component. Repeat it for Right Controller.
{{< figure src="ovr.png" height=400 align=center  >}}

We use our old stuffs from roll-a-ball. Resize and place it on the table. For interactions, to avoid problems with some colliders, we create different layers so that the controller's selection does not affect other colliders.

We put Player, Pickups, Walls and Ground and children into the **roll-a-ball** layer. Add colliders to roll-a-ball and two HandAnchor and put them into the **selection** layer. For the **layer collision matrix**, selection objects should not trigger roll-a-ball.

Finally, add a new script MySelect.cs on both HandAnchor.
```
if (controller is in the collider of roll-a-ball)
    if (not selected and pull the trigger)
        selects roll-a-ball
    else if (selected and release the trigger)
        releases roll-a-ball
```
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MySelect : MonoBehaviour
{
    public OVRInput.Controller controller;
    private float triggerValue;
    [SerializeField] private bool isInCollider;
    [SerializeField] private bool isSelected;
    private GameObject selectdObj;

    // Update is called once per frame
    void Update()
    {
        triggerValue = OVRInput.Get(OVRInput.Axis1D.PrimaryIndexTrigger, controller);

        if (isInCollider)
        {
            if(!isSelected&&triggerValue > 0.95f)
            {
                isSelected = true;
                selectdObj.transform.parent = this.transform;
                Rigidbody rb = selectdObj.GetComponent<Rigidbody>();
                rb.isKinematic = true;
                rb.useGravity = false;
                rb.velocity = Vector3.zero;
                rb.angularVelocity = Vector3.zero;
            }
            else if (isSelected && triggerValue < 0.95f)
            {
                isSelected = false;
                selectdObj.transform.parent = null;
                Rigidbody rb = selectdObj.GetComponent<Rigidbody>();
                rb.isKinematic = false;
                rb.useGravity = true;
                rb.velocity = OVRInput.GetLocalControllerVelocity(controller);
                rb.angularVelocity = OVRInput.GetLocalControllerAngularVelocity(controller);
            }
        }
    }

    void OnTriggerEnter(Collider other)
    {
        if(other.gameObject.name == "roll-a-ball")
        {
            isInCollider = true;
            selectdObj = other.gameObject;
        }
    }

    void OnTriggerExit(Collider other)
    {
        if(other.gameObject.name == "roll-a-ball")
        {
            isInCollider = false;
            selectdObj = null;
        }
    }
}

```
And we can use Interaction SDK to add interactions like ray, poke, locomotion, and grab for controllers, hands, and controllers as hands.
## Deploy
Enable developer mode on Quest to build and run project directly.

{{< figure src="video.gif" height=400 align=center  >}}



