---
title: "Mars Lander 2 trigonometry - C# tutorial - CodinGame"
date: 2020-08-15T20:45:23+07:00
draft: false
tags: [Csharp, CodinGame, Difficult]
featured_image: "https://i2.ytimg.com/vi/605RHv7sBCI/maxresdefault.jpg"
description: "We come back to the Mars Lander 2 puzzle to add a trigonometric solution"
---
Topics: 

explanation of the trigonometry calculation of the landing angle

ArcoSine is the function to get the angle from the sine (desired acceleration)

conversion from angle in radians to angle in degrees

{{< youtube 605RHv7sBCI >}}

## Solution {#solution}
Spoiler alert: first try to solve the problem by yourself. Or, try to write the code by yourself, while following the video tutorial.

{{< highlight csharp "linenos=table,hl_lines=7 73-80,linenostart=1" >}}
using System;                       // used by Math and by Console
using System.Collections.Generic;   // used by List<int>
using System.Linq;                  // used by Select() to parse inputs

class MarsLander
{
    const bool UseTrigonometry = true; // if false, there is a simple approximation to calculate the angle, given the acceleration

    static int x;      //  lander current X
    static int hSpeed; //  lander current horizontal velocity
    static int vSpeed; //  lander current vertical velocity
    static (int left, int mid, int right) flat = (0,0,0);  // flat landing surface on Mars
    static int outputAngle = 0;
    static int outputThrust = 4;

    const int maxHorizonalSpeed = 100, desiredSpeedWhenNear = 20;
    const int farDistance = 2000;   // distance from landing area considered "far"
    static int  distanceX => Math.Abs(flat.mid - x);
    static bool isFar => (distanceX > farDistance);
    static bool isOverLandingArea => (x >= flat.left) && (x <= flat.right);
    static int  desiredDirection => Math.Sign(flat.mid - x);  // -1=left, +1=right, 0=don't move horizontally
    static int  currentDirection => Math.Sign(hSpeed);  // -1=left, +1=right, 0=not moving horizontally
    static bool isMovingToDesiredDirection => (desiredDirection == currentDirection);
    static bool isMovingUp => (vSpeed > 0);

    static void Main()
    {
        ReadMarsSurface();
        
        //first update
        ReadUpdateInputs();
        Console.WriteLine("0 0"); // rotate power
        int desiredInitialSpeed = isMovingToDesiredDirection ? Math.Abs(hSpeed) : maxHorizonalSpeed;

        // game loop
        while (true)
        {
            ReadUpdateInputs();
            outputThrust = 4;

            if (isOverLandingArea)
            {
                bool landingSpeedIsOk = Math.Abs(vSpeed) < 40;
                if (landingSpeedIsOk && hSpeed == 0)
                    outputThrust = 3;
                outputAngle = GetDesiredAngle(desiredSpeedX:0, maxAngle:33);
            }
            else
            {
                if (isMovingUp)
                    outputThrust = 3;
                if (isFar)
                    outputAngle = GetDesiredAngle(desiredInitialSpeed, maxAngle:60);
                else // lander is near but not over landing area
                    outputAngle = GetDesiredAngle(desiredSpeedWhenNear, maxAngle:45);
            }

            Console.WriteLine($"{outputAngle} {outputThrust}");
        }
    }

    static int GetDesiredAngle(int desiredSpeedX, int maxAngle)
    {
        int desiredVelocity = desiredSpeedX * desiredDirection;
        int desiredAcceleration = desiredVelocity - hSpeed;

        int idealAngle = UseTrigonometry ? GetAngleWithTrigonometry(desiredAcceleration) :
                            GetAngleUsingApproximation(desiredAcceleration);

        return -Math.Clamp(idealAngle, min:-maxAngle, max:+maxAngle);
    }

    static int GetAngleWithTrigonometry(int desiredAcceleration)
    {
        double achievableAcceleration = Math.Clamp(desiredAcceleration, min: -outputThrust, max: outputThrust); // acceleration possible between -90 to +90 degrees
        double desiredSine =  achievableAcceleration / outputThrust; // -1 to +1
        double angleInRadians = Math.Asin(desiredSine); // ArcSine is the inverse function of Sine
        double angleInDegrees = angleInRadians / Math.PI * 180;
        return (int) angleInDegrees;
    }

    static int GetAngleUsingApproximation(int desiredAcceleration)
    {
        return desiredAcceleration * 2;
    }

    static void ReadMarsSurface()
    {
        // X coordinate of a surface point. (0 to 6999)
        // Y coordinate of a surface point. 
        // By linking all the points together in a sequential fashion, you form the surface of Mars.

        int surfacePointCount = int.Parse(Console.ReadLine()); // the number of points used to draw the surface of Mars.
        var points = new List<(int x, int y)>();
        for (int i = 0; i < surfacePointCount; i++)
        {
            List<int> inputs = ReadLineOfInts();
            points.Add( (inputs[0], inputs[1]) );
            bool isFlat = i>0 && (points[i].y == points[i-1].y);
            if (isFlat)
            {
                flat.left = points[i-1].x;
                flat.right = points[i].x;
                flat.mid = (flat.left + flat.right) / 2;
                Console.Error.WriteLine($"flat_left = {flat.left}; flat_right = {flat.right}; flat_height = {points[i].y}; ");
            }
        }

        var test = points.Select(point => $"({point.x},{point.y})").ToList();
        Console.Error.WriteLine($"mars = [{string.Join(',', test)} ]");


    }

    static void ReadUpdateInputs()
    {
        List<int> inputs = ReadLineOfInts();
        x = inputs[0];
        //int y = inputs[1];
        hSpeed = inputs[2]; // the horizontal velocity (in m/s), can be negative.
        vSpeed = inputs[3]; // the vertical velocity (in m/s), can be negative.
        //int fuel = inputs[4]; // the quantity of remaining fuel in liters.
        //int rotate = inputs[5]; // the rotation angle in degrees (-90 to 90).
        //int power = inputs[6]; // the thrust power (0 to 4).
  	    Console.Error.WriteLine($"isFar:{isFar}, isOverLandingArea:{isOverLandingArea}, isCorrectDirection:{isMovingToDesiredDirection}");
    }

    static List<int> ReadLineOfInts()
    {
        return Console.ReadLine().Split(' ').Select(int.Parse).ToList();
    }
}
{{< / highlight >}}