# Step 4

## Moving the Character with Button Inputs - Overview

In the previous chapter, we breathed some life into Glod and transformed him from a static image into a fully animated character.

Amazing!

However, we can take things further. Let's free Glod from his stationary curse and make him move! To do this we will:

1. Place a variable in the `Position` tag of `UI.Area`.
2. Change this variable to make Glod move.
3. Use velocity and acceleration concepts to make Glod jump.

## Step 4.1

First, let's edit our code a little. Currently, Glod is situated in the center of the screen. While this can work, we can instead change his current `Anchor.Center` to `Anchor.Top`, and bring him down to Earth by changing his position. We achieve this by creating some variables.

??? ":bulb:"
    While any anchor will work as long as we change the position accordingly, we will be using `Anchor.Top` to keep things consistent and manage the positions of the different game elements more effectively.

Create the `.x` and `.y` variables which will be used to create the variable `.character-position`.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    0.0 >= .x ;; (1)
    310.0 >= .y
    (float2 .x .y) >= .character-position
    ```

    1. Added to line 13 under `initialize-character`.

`.character-position` is then fed into the `Position` parameter of the `UI.Area` shard. When the `.x` and `.y` values are changed, the `UI.Area` will move and hence move our character.

Remember to change the `Anchor` to `Anchor.Top`!

=== "Code Added"
    ```{.clojure .annotate linenums="1"}
    (UI.Area
     :Position .character-position 
     :Anchor Anchor.Top ;; (1)
     :Contents
    ```
    
    1. Code edited at line 152.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ----------
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state
        1 > .character-direction))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (-> 
        (Msg "up") 
        3 > .character-state))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state)))

    (defloop main-wire
      (Setup
       (initialize-character))

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 4.2

Now that our variables are set up, it's time to make Glod move! Let's start by moving him to the left and right.

To do this, we simply have to change the `.x` value that we have just created whenever the left and right buttons are pressed.

First, create a new variable named `.character-x-velocity`. We will change this value when the right and left directional buttons are pressed to make our character move.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    0.0 >= .character-x-velocity ;; (1)
    ```

    1. Added to line 16 under `initialize-character`.

Create the `run-logic` shard. When `.character-x-velocity` is changed, it will be added to `.x` and `.character-position` will be updated accordingly.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------ Character Run Logic ----------------
    (defshards run-logic [] ;; (1)
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position)
    ```

    1. Added to line 81.

 Update the value of `.character-x-velocity` whenever the left or right directional buttons are pressed. Remember to reset the value back to 0 when the buttons are released. If not, Glod will move to the left or right forever.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    ;; ------- Button Inputs ----------
    (defshards button-inputs [] ;; (1)
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (-> 
        (Msg "up") 
        3 > .character-state))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))
    ```

    1. Updated within the `button-inputs` shard at line 89.

    ```{.clojure .annotate linenums="1"}
    (run-logic) ;; (1)
    ```

    1. `run-logic` is added to the `main-wire` at line 133.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ---------- 
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position
      0.0 >= .character-x-velocity

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position)

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (-> 
        (Msg "up") 
        3 > .character-state))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))

    (defloop main-wire
      (Setup
       (initialize-character))
      
      (run-logic)

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 4.3

Oh no! We now have a problem. 🥲

If we continue moving Glod left or right, you might notice that he goes off-screen.

Let's set up a boundary to ensure that Glod does not fall off the edge and into the abyss. To do this, create a clamp function that limits how far our x value can go.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ---------- Character Boundary ------------ ;; (1)
    (defshards clamp [var min max]
      var (Max min) (Min max) > var)
    ```

  1. Added to line 81.

This clamp function takes in a value and makes sure that it does not exceed the minimum and maximum values.

Call our `clamp` function in `run-logic` and set the `var` parameter to be `.x`, the `min` parameter to be -600, and the `max` parameter to be 600. 

!!! note
  You can change the `min` and `max` parameters to fit your screen accordingly.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (clamp .x -600.0 600.0) ;; (1)
    ```

    1. Added to line 92.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ----------
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position
      0.0 >= .character-x-velocity

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- Character Boundary ------------
    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position
      
      (clamp .X -600.0 600.0))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (-> 
        (Msg "up") 
        3 > .character-state))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))

    (defloop main-wire
      (Setup
       (initialize-character))
      
      (run-logic)

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction 
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2))) 
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))] 
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Yay! Now Glod will not walk off into the void! 😙

## Step 4.4

Now, let's make Glod jump!

Instead of just using velocity, we can add acceleration into the mix. This will make Glod's jump feel more amazing.

??? "Jumping using Velocity & Acceleration"
    For smooth realistic jumping, we have to decrease velocity by acceleration whenever we jump. Decreasing velocity by acceleration decreases the rate of increase of `.y`.
    
    This simulates gravity, making our jump feel more realistic.

First, create the variables that we will use.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    0.0 >= .character-y-velocity ;; (1)
    0.0 >= .character-y-acceleration
    ```

    1. Added to lines 17 and 18.

Similar to `run-logic`, we will add `.character-y-velocity` to our `.y` variable. This time however, we will also add .`character-y-acceleration` to `.character-y-velocity`.

=== "Code Added"
    ```{.clojure .annotate linenums="1"}
    ;; ------------ Character Gravity Logic ---------------
    (defshards gravity-logic [] ;; (1)
      .y (Math.Add .character-y-velocity)
      > .y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .x .y) > .character-position)
    ```

    1. Added to line 97.

    ```{.clojure .annotate linenums="1"}
    (gravity-logic) ;; (1)
    ```

    1. Added to line 154 in `main-wire`.

Lastly we modify the value of `.character-y-velocity` and `.character-y-acceleration` whenever the Up directional button is pressed.

=== "Code Added"
    ```{.clojure .annotate linenums="1"}
    (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                -20.0 > .character-y-velocity ;; (1)
                1.0 >  .character-y-acceleration))
    ```
    1. Added to line 132 in `button-inputs`.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ---------- 
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- Character Boundary ------------
    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position
      
      (clamp .X -600.0 600.0))

    ;; ------------ Character Gravity Logic ---------------
    (defshards gravity-logic []
      .y (Math.Add .character-y-velocity)
      > .y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .x .y) > .character-position)

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state
        -20.0 > .character-y-velocity
        1.0 >  .character-y-acceleration))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))

    (defloop main-wire
      (Setup
       (initialize-character))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction 
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2))) 
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))] 
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

??? ":bulb:"
    Since our `Anchor` is set to `Anchor.Top`, it places the `UI.Area` at the top and in the middle of the screen. This is now our origin at (0,0).
    
    In Shards, increasing the value of the y-axis pushes our `UI.Area` down while decreasing it pushes it up.

    This is why our velocity is a negative value. When added to our `.y` value, it decreases `.y` and makes our image move upwards.
    
    Similarly, our acceleration is a positive value so that when it gets added to our negative value velocity, it decreases it.

## Step 4.5

Try running your code now! 

Oh no, we have a problem. Glod falls through the floor! 😨

Don't worry, we can fix this!

Similar to how we made a boundary to ensure that Glod does not walk off-screen, we can add a boundary to ensure he doesn't fall through the floor by reusing the same `clamp` function we made in the previous step.

First, call our `clamp` function in `gravity-logic` with `.y` as our `var`, -620 as our `min`, and 620 as our `max`.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (clamp .y -620.0 620.0) ;; (1)
    ```

    1. Added to lines 106.

Next, add a conditional statement to ensure that our y velocity and acceleration reverts back to 0 when Glod is on the ground.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    .y ;; (1)
    (When
     :Predicate (IsMoreEqual 620.0)
     :Action (->
              0.0 > .character-y-velocity
              0.0 > .character-y-acceleration))
    ```

    1. Added to line 107.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ---------- 
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- Character Boundary ------------
    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position
      
      (clamp .X -600.0 600.0))

    ;; ------------ Character Gravity Logic ---------------
    (defshards gravity-logic []
      .y (Math.Add .character-y-velocity)
      > .y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .x .y) > .character-position

      (clamp .y -620.0 620.0)
      .y
      (When
       :Predicate (IsMoreEqual 620.0)
       :Action (->
                0.0 > .character-y-velocity
                0.0 > .character-y-acceleration)))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state
        -20.0 > .character-y-velocity
        1.0 >  .character-y-acceleration))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))

    (defloop main-wire
      (Setup
       (initialize-character))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction 
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2))) 
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))] 
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Congratulations! Glod can now move and jump safely! 😊

## Step 4.6

Finally, let's polish up the code a little.

You might have noticed other small problems: 

1. Glod does not go back to his idle animation when he lands on the floor.
2. Glod changes to his running animation if we move left and right while jumping.
3. Glod can continuously jump as long as you press the jump button.

Let's fix these problems!

For the first issue, we simply have to ensure that Glod goes back to his idle animation when he reaches the floor.

In `gravity-logic`, revert `character-state` to 0 when the character is jumping.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
      .y
      (When
       :Predicate (IsMoreEqual 620.0)
       :Action (->
                0.0 > .character-y-velocity
                0.0 > .character-y-acceleration
                .character-state ;; (1)
                (When
                 :Predicate (Is 3)
                 :Action (->
                          0 > .character-state))))
    ```

    1. Added to line 113, under `gravity-logic`.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ---------- 
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- Character Boundary ------------
    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position
      
      (clamp .X -600.0 600.0))

    ;; ------------ Character Gravity Logic ---------------
    (defshards gravity-logic []
      .y (Math.Add .character-y-velocity)
      > .y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .x .y) > .character-position

      (clamp .y -620.0 620.0)
      .y
      (When
       :Predicate (IsMoreEqual 620.0)
       :Action (->
                0.0 > .character-y-velocity
                0.0 > .character-y-acceleration
                .character-state
                (When
                 :Predicate (Is 3)
                 :Action (->
                          0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state
        -20.0 > .character-y-velocity
        1.0 >  .character-y-acceleration))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))

    (defloop main-wire
      (Setup
       (initialize-character))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction 
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2))) 
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))] 
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

??? ":bulb:"
    Here, we use a nested `When` conditional statement to ensure that Glod goes back to his idle animation only when he is jumping.
    
    Had we simply put `0 >. character-state` without the nested `When`, this would revert Glod back to his idle animation but it would cause another problem. That is, Glod will forever be in his idle animation whenever he is on the ground. This means that he will never transit into his walking animation.
    
    This is why we use the nested `When` conditional statement instead.

The second problem is also an easy fix. We simply put a `When` conditional statement to make Glod go into his walking animation when he is idling (`.character-state` is 0) and not jumping.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (Inputs.KeyDown
     :Key "left"
     :Action
     (->
      (Msg "left")
      .character-state ;; (1)
      (When
       :Predicate (Is 0)
       :Action (-> 1 > .character-state))
      0 > .character-direction
      -5.0 > .character-x-velocity))

    (Inputs.KeyDown
     :Key "right"
     :Action
     (->
      (Msg "right")
      .character-state
      (When
       :Predicate (Is 0)
       :Action (-> 2 > .character-state))
      1 > .character-direction
      5.0 > .character-x-velocity))
    ```

    1. Edited from line 126 onwards.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ---------- 
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- Character Boundary ------------
    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position
      
      (clamp .X -600.0 600.0))

    ;; ------------ Character Gravity Logic ---------------
    (defshards gravity-logic []
      .y (Math.Add .character-y-velocity)
      > .y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .x .y) > .character-position

      (clamp .y -620.0 620.0)
      .y
      (When
       :Predicate (IsMoreEqual 620.0)
       :Action (->
                0.0 > .character-y-velocity
                0.0 > .character-y-acceleration
                .character-state
                (When
                 :Predicate (Is 3)
                 :Action (->
                          0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        .character-state
        (When
         :Predicate (Is 0)
         :Action (-> 1 > .character-state))
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        .character-state
        (When
         :Predicate (Is 0)
         :Action (-> 2 > .character-state))
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state
        -20.0 > .character-y-velocity
        1.0 >  .character-y-acceleration))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))

    (defloop main-wire
      (Setup
       (initialize-character))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction 
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2))) 
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))] 
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

For the last problem, although slightly more complicated, we will employ the `When` conditional statement too to solve it.

First, create a `.can-jump` variable under `initialize-character`.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    true >= .can-jump ;; (1)
    ```

    1. Added to line 12.

Set it such that Glod cannot jump again when he is jumping. When the jump button is pressed, `.can-jump` becomes false.

The increase in velocity and acceleration to make Glod jump only happens when `.can-jump` is true.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    (Inputs.KeyDown
     :Key "up"
     :Action
     (->
      (Msg "up")
      3 > .character-state
      .can-jump ;; (1)
      (When
       :Predicate (Is true)
       :Action (->
                -20.0 > .character-y-velocity
                1.0 >  .character-y-acceleration
                false >= .can-jump))))
    ```

    1. Edited from line 153 onwards.

Lastly we reset `.can-jump` back to true when Glod touches the floor again.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    .y
    (When
     :Predicate (IsMoreEqual 620.0)
     :Action (->
              0.0 > .character-y-velocity
              0.0 > .character-y-acceleration
              true > .can-jump ;; (1)
              .character-state
              (When
               :Predicate (Is 3)
               :Action (->
                        0 > .character-state))))
    ```

    1. Edited from line 114 onwards.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      ;; -------------- Character Jumping  ----------
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction ;; 0 = facing left, 1 = facing right
      true >= .can-jump

      0.0 >= .x
      310.0 >= .y
      (float2 .x .y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left)----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed ;; Reduce number to increase animation speed
      )
    
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- Character Boundary ------------
    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .x (Math.Add .character-x-velocity)
      > .x

      (float2 .x .y) > .character-position
      
      (clamp .X -600.0 600.0))

    ;; ------------ Character Gravity Logic ---------------
    (defshards gravity-logic []
      .y (Math.Add .character-y-velocity)
      > .y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .x .y) > .character-position

      (clamp .y -620.0 620.0)
      .y
      (When
       :Predicate (IsMoreEqual 620.0)
       :Action (->
                0.0 > .character-y-velocity
                0.0 > .character-y-acceleration
                true > .can-jump
                .character-state
                (When
                 :Predicate (Is 3)
                 :Action (->
                          0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        .character-state
        (When
         :Predicate (Is 0)
         :Action (-> 1 > .character-state))
        0 > .character-direction
        -5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        .character-state
        (When
         :Predicate (Is 0)
         :Action (-> 2 > .character-state))
        1 > .character-direction
        5.0 > .character-x-velocity))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state
        .can-jump
        (When
         :Predicate (Is true)
         :Action (->
                  -20.0 > .character-y-velocity
                  1.0 >  .character-y-acceleration
                  false >= .can-jump))))

      (Inputs.KeyUp
       :Key "left"
       :Action 
       (->
        0 > .character-state
        0.0 > .character-x-velocity))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state
        0.0 > .character-x-velocity)))

    (defloop main-wire
      (Setup
       (initialize-character))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (GFX.MainWindow
       :Title "MainWindow" :Width 1920 :Height 1080
       :Contents
       (->
        (Setup
         (GFX.DrawQueue) >= .ui-draw-queue
         (GFX.UIPass .ui-draw-queue) >> .render-steps)
        .ui-draw-queue (GFX.ClearQueue)

        (UI
         .ui-draw-queue
         (->
          (UI.Area
           :Position .character-position
           :Anchor Anchor.Top
           :Contents
           (->
            .character-state
            (Match [0 (-> .character-direction
                          (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                  1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                 :Passthrough false))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (->  .character-direction
                          (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                  1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                  :Passthrough false))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Recap

Try running your code now! Glod should be happily running and jumping without any glitches. 😁

To recap:

1. We made Glod move by tying a variable to the `UI.Area` position and changing said variable.

2. We ensured that Glod does not walk or fall into the void by creating boundaries.

3. We created conditional statements to fix several problems.

And walah! We now have a fully functional moving character!

In the next chapter, let's have Glod collect some coins! 🤑
