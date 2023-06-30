# Step 5

## Dropping Coins and Scoring Points - Overview

Good job on reaching this far!

Now that we have a moving character, we can add more game elements.

As mentioned in the intro, this will be a point collection game. We'll start by creating coins for Glod to collect.

In this chapter we will be:

1. Drawing a coin image on screen.

2. Having it move by using variables.

3. Creating a spawning system to randomly spawn coins on the screen.

4. Creating a collision system to have Glod be able to collect coins.

5. Reusing code to create more coins by making shards accept arguments.

## Step 5.1

Drawing an image on screen and using variables to move them... Does that not sound familiar? 🤔

Yes! It is using the same logic to make Glod appear on the screen. Download the images that we will be using:

- Download Coin Images [here](https://drive.google.com/file/d/1zqdvZtocDsU9mI_VmN4oqpAYF6L5vSzJ/view?usp=share_link).

Let's start off nice and easy by first drawing the coin image on the screen.

Create a new `initialize-coin` shard. We will separate the initializing of our coin and character variables to make our code neater.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin [] ;; (1)
      (LoadTexture "GlodImages/Coin/Coin_1.png") = .coin-1)
    ```

    1. Added to line 176.

Remember to call `initialize-coin` in the `Setup` of the `main-wire`to ensure that these variables are only created once.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (defloop main-wire
      (Setup
       (initialize-character)
       (initialize-coin)) ;; (1)
    ```

    1. Added to line 182.

We then create a new `UI.Area` to draw the coin on the screen.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (UI.Area ;; (1)
     :Position (float2 0 0)
     :Anchor Anchor.Top
     :Contents (-> .coin-1 (UI.Image :Scale (float2 0.2)))
    ```

    1. Added to line 220.

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

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") = .coin-1)

    (defloop main-wire
      (Setup
       (initialize-character)
       (initialize-coin))
      
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
                   :Passthrough false)))

          (UI.Area
           :Position (float2 0 0)
           :Anchor Anchor.Top
           :Contents (-> .coin-1 (UI.Image :Scale (float2 0.2))))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 5.2

Try running your code now. You should see a coin being drawn on the top of the screen.

Congrats!

We are off to a good start. Now, let's animate this coin.

The logic will be exactly the same as when we animated Glod. This will be much easier however as we won't need any conditional statements to control the coin.

- Download coin images [here](https://drive.google.com/drive/folders/1_iAhj70R3kS5hBXKtqh67Cfb_jBKRlzh?usp=share_link).

Just like what we did to animate Glod, we replace the singular image with an array of images.

We also create variables for controlling the Coin animation:

- `.coin-image-index`

- `.coin-image-index-max`

- `.coin-animation-speed`

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin [] ;; (1)
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) (Math.Subtract 1) >= .coin-image-index-max
      0 >= .coin-image-index
      0.1 >= .coin-animation-speed)
    ```

    1. Updated `initialize-coin` at line 176.

Then we create a `coin-animation` loop that follows the same logic as Glod's animations.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    ;; -------------- Coin Animation ------------------
    (defloop coin-animation ;; (1)
      .coin-image-index (Math.Add 1)
      .coin-image-index
      (When
       :Predicate (IsMore .coin-image-index-max)
       :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed)) 
    ```

    1. Added to line 189.

Remember to `Step` into the Coin's animation in your `main-wire`!

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    (Step coin-animation) ;; (1)
    ```

    1. Added to line 209.

Lastly, update your coin's `UI.Area` to draw the image located in an index of `.coin-image-array`. The index used is determined by the value of `.coin-image-index`.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    (UI.Area
     :Position (float2 0 0)
     :Anchor Anchor.Top
     :Contents
     (-> .coin-image-array
         (Take .coin-image-index)
         (UI.Image :Scale (float2 0.2))))
    ```

    1. Added to line 241.

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

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) (Math.Subtract 1) >= .coin-image-index-max
      0 >= .coin-image-index
      0.1 >= .coin-animation-speed)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When
       :Predicate (IsMore .coin-image-index-max)
       :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))   
    
    (defloop main-wire
      (Setup
       (initialize-character)
       (initialize-coin))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)

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
                   :Passthrough false)))

          (UI.Area
           :Position (float2 0 0)
           :Anchor Anchor.Top
           :Contents
           (-> .coin-image-array
               (Take .coin-image-index)
               (UI.Image :Scale (float2 0.2))))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Try running your code to see if your coin is animated. Good job if it is!

## Step 5.3

Now let's make our coin fall. To do this, like we did with Glod, we will:

1. Set a variable in the `Position` parameter of the coin's `UI.Area`, replacing the `(float2 0 0)` used previously.

2. Change this variable accordingly.

Without further ado, let's get started!

First we create the coin's x, y, and position variables.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ----- Coin 1 ------
    0.0 >= .coinx-1 ;; (1)
    0.0 >= .coiny-1
    (float2 .coinx-1 .coiny-1) >= .coin-position-1
    ```

    1. Added to line 189.

We then replace the value of the `Position` parameter in our coin's `UI.Area`.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (UI.Area
     :Position .coin-position-1 ;; (1)
     :Anchor Anchor.Top
     :Contents
     (-> .coin-image-array
         (Take .coin-image-index)
         (UI.Image :Scale (float2 0.2))))
    ```

    1. Amended at line 247.


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

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) (Math.Subtract 1) >= .coin-image-index-max
      0 >= .coin-image-index
      0.1 >= .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When
       :Predicate (IsMore .coin-image-index-max)
       :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))   
    
    (defloop main-wire
      (Setup
       (initialize-character)
       (initialize-coin))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)

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
                   :Passthrough false)))

          (UI.Area
           :Position .coin-position-1
           :Anchor Anchor.Top
           :Contents
           (-> .coin-image-array
               (Take .coin-image-index)
               (UI.Image :Scale (float2 0.2))))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Try changing the `.coinx-1` or `.coiny-1` values to check if your coin's position changes. Wonderful!

Now, similar to our running and jumping logic, we have to create a gravity logic to make our coin fall. Let's once again use velocity and acceleration to make our coin fall realistically.

First, create velocity and acceleration variables under the `initialize-coin` shard.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    0.0 >= .coin-velocity-1 ;; (1)
    0.5 >= .coin-acceleration
    ```

    1. Added to line 187, under `initialize-coin`.

Create our `coin-gravity-logic` shard. The logic behind it is the same as our jump logic. We add velocity to the y position and then add acceleration to our velocity to increase the rate of increase.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [] ;; (1)

      .coiny-1 (Math.Add .coin-velocity-1)
      > .coiny-1

      .coin-velocity-1 (Math.Add .coin-acceleration)
      > .coin-velocity-1

      (float2 .coinx-1 .coiny-1) > .coin-position-1)
    ```

    1. Added to line 206.

Remember to call the `coin-gravity-logic` shard in our `main-wire` loop.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (coin-gravity-logic) ;; (1)
    ```

    1. Added to line 228.

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

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) (Math.Subtract 1) >= .coin-image-index-max
      0 >= .coin-image-index
      0.1 >= .coin-animation-speed
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When
       :Predicate (IsMore .coin-image-index-max)
       :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))   
    
    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic []

      .coiny-1 (Math.Add .coin-velocity-1)
      > .coiny-1

      .coin-velocity-1 (Math.Add .coin-acceleration)
      > .coin-velocity-1

      (float2 .coinx-1 .coiny-1) > .coin-position-1)

    (defloop main-wire
      (Setup
       (initialize-character)
       (initialize-coin))
      
      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (coin-gravity-logic)

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
                   :Passthrough false)))

          (UI.Area
           :Position .coin-position-1
           :Anchor Anchor.Top
           :Contents
           (-> .coin-image-array
               (Take .coin-image-index)
               (UI.Image :Scale (float2 0.2))))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 5.4

If you run your code now, you would see our coin falling! Nice. 😁

Now let's have adorable Glod chase some money! 🤑💰💵💶💷

To do that, we need to create the logic to:

1. Check when Glod is colliding with our coin.

2. Gain a point when the collision happens.

Let's do this step by step.

First, nice and easy, let's create a `.score` variable. Remember to initialize this in your main loop setup.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (defshards initialize-game-elements []
      0 >= .score)
    ```
    ```{.clojure .annotate linenums="1"}
    (initialize-game-elements)
    ```

Next, create a new `UI.Area` to draw our `.score` using `UI.Label`. Remember to convert our `.score` to a string first with the `(ToString)` shard.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (UI.Area :Position (float2 -40 20)
                                :Anchor Anchor.TopRight
                                :Contents (->
                                          .score (ToString)(UI.Label)))
    ```

If you try to run our code now you will see a teeny tiny 0 in the corner of your screen. This means that our score is being drawn! But it unfortunately has not been styled yet. So let's do that.

We first create a style shard and then call this shard before we draw our `UI.Label`. Now if you try running your code you would see that our score is styled nicely.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})
    ```
    ```{.clojure .annotate linenums="1"}
    ;; ------ UI Style --------
    (UI.Area :Position (float2 -40 20)
                                :Anchor Anchor.TopRight
                                :Contents (->
                                          style (UI.Style)
                                          .Score (ToString)(UI.Label))) ;;(1)
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
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
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1

      0.5 >= .coin-acceleration)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic []

      .coiny-1 (Math.Add .coin-velocity-1)
      > .coiny-1

      .coin-velocity-1 (Math.Add .coin-acceleration)
      > .coin-velocity-1

      (float2 .coinx-1 .coiny-1) > .coin-position-1)

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score)

    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements))

      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (coin-gravity-logic)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
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
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 5.5

Now that we have our score UI, we can start to increase it. To do this, we need our collision logic.

Create:

1. An upper and lower x limit.

2. An upper and lower y limit.

This is to draw a collision box around our dear Glod. These variables are added to our `initialize-game-elements` shard. Since our character is moving, we have to constantly update our collision box.

[Glod Collision Box](assets/step5_5_Image.png)

We also create a `.scored` variable to limit the number of times we can score from one coin. These variables will be added under `initialize-game-elements`.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    .X (Math.Add 50.0)
    >= .scoringUpper-x-limit
    .X (Math.Subtract 50.0)
    >= .scoringLower-x-limit

    .Y (Math.Add 10.0)
    >= .scoringUpper-y-limit
    .Y (Math.Subtract 10.0)
    >= .scoringLower-y-limit
    
    false >= .scored
    ```

Next, we will implement our collision logic with a conditional statement.

Create the logic to trigger whenever our coin falls within our collision box.

This conditional statement will check if the x value of our coin:

- Is lesser then the upper x limit of our collision box.

- Is more than the lower x limit of our collision box.

It is checking if it within the stated range and does the same for the y value. Next, we increase the score whenever this happens.

Whenever the `.scored` variable is true, gain a point. Once a point is gained, Reset `.scored` to false once again. This ensures that only one point is gained whenever we collect a coin. Remember to call the `(scoring)` shard in your main wire

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------- Scoring ------------
    (defshards score-collision []
      .coinx-1
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        .coinx-1 (IsMore .scoringLower-x-limit)
                        (And)
                        .coiny-1 (IsLess .scoringUpper-y-limit)
                        (And)
                        .coiny-1 (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit 

      (score-collision)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
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
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1

      0.5 >= .coin-acceleration)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic []

      .coiny-1 (Math.Add .coin-velocity-1)
      > .coiny-1

      .coin-velocity-1 (Math.Add .coin-acceleration)
      > .coin-velocity-1

      (float2 .coinx-1 .coiny-1) > .coin-position-1)

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score
      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit

      false >= .scored)

    ; ------------- Scoring ------------
    (defshards score-collision []
      .coinx-1
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        .coinx-1 (IsMore .scoringLower-x-limit)
                        (And)
                        .coiny-1 (IsLess .scoringUpper-y-limit)
                        (And)
                        .coiny-1 (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements))

      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (coin-gravity-logic)
      
      (scoring)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
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
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Try running your code now and see if you get a point. You did!? Congratulations!

# Step 5.6

Now that we have a scoring system, let's create more coins.

Currently, we have only one coin falling down. Let's reuse this coin and have it appear again in another random x position for Glod to run after and collect.

Here we are creating a loop that will give our coin a random x value and reset its y value. It will do this every one and a half seconds. This means that our coin will randomly spawn, fall, and then one and a half seconds later, randomly spawn and fall again.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ---------- Random Coin ------------
    (defloop random-coin
      .coinx-1
      (RandomFloat :Max 1200.0)
      > .coinx-1
      (Math.Subtract 600.0)
      > .coinx-1

      0.0 > .coiny-1
      0.0 > .coin-velocity-1
      (float2 .coinx-1 .coiny-1) > .coin-position-1
      (Pause 1.5))
    ```

Remember to `Step` into your loop in your main wire.

    ```{.clojure .annotate linenums="1"}
    (Step  random-coin) ;; (1)      
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
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
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1

      0.5 >= .coin-acceleration)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic []

      .coiny-1 (Math.Add .coin-velocity-1)
      > .coiny-1

      .coin-velocity-1 (Math.Add .coin-acceleration)
      > .coin-velocity-1

      (float2 .coinx-1 .coiny-1) > .coin-position-1)

    ;; ------------- Random Coin ------------------
    (defloop random-coin
      .coinx-1
      (RandomFloat :Max 1200.0)
      > .coinx-1
      (Math.Subtract 600.0)
      > .coinx-1

      0.0 > .coiny-1
      0.0 > .coin-velocity-1
      (float2 .coinx-1 .coiny-1) > .coin-position-1
      (Pause 1.5))

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score
      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit

      false >= .scored)

    ; ------------- Scoring ------------
    (defshards score-collision []
      .coinx-1
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        .coinx-1 (IsMore .scoringLower-x-limit)
                        (And)
                        .coiny-1 (IsLess .scoringUpper-y-limit)
                        (And)
                        .coiny-1 (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements))

      (run-logic)
      (gravity-logic)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (coin-gravity-logic)
      (Step  random-coin)
      
      (scoring)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
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
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Try running your code now!

Congrats, you have a very basic game. You can try it out and have fun chasing coins! 😁

## Step 5.7

While we have a basic game on our hands now, we can still polish it further. Currently, we only have one coin spawning. Let's have more! To do this, we have to change some of our shards to accept arguments instead of having hard coded variables.

Create some new variables for our second coin. These variables are added under `initialize-coin`.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)
    ```

Next, create a `UI.Area` to draw our new coin. You can try running your code at this point to make sure that your coin is being drawn.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))
    ```

Now to make it fall...

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ###### DON'T DO THIS ########
    (defshards coin-gravity-logic []

      .coiny-1 (Math.Add .coin-velocity-1)
      > .coiny-1

      .coin-velocity-1 (Math.Add .coi-acceleration)
      > .coin-velocity-1

      (float2 .coinx-1 .coiny-1) > .coin-position-1)

    (defshards coin-gravity-logic-2 []

      .coiny-2 (Math.Add .coin-velocity-1)
      > .coiny-2

      .coin-velocity-2 (Math.Add .coin-acceleration)
      > .coin-velocity-2

      (float2 .coinx-2 .coiny-2) > .coin-position-2)

    ```

While the above code can work, our code will end up being very messy. Instead, let's have our original `coin-gravity-logic` shard take in arguments instead.

Here we convert our original `coin-gravity-logic` shard into one that takes arguments. Now that we have this, rather than duplicating this code a million times, we can call it as many times as required and simply pass it the correct variables.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)
    ```

    ```{.clojure .annotate linenums="1"}
    (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
    (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)
    ```

We'll do the same for our coin randomizing logic.

Now you might be wondering how do we get our `random-coin` logic to accept arguments as it is a `defloop` and not a `defshards`.

Do not fret!

To do this, we simply change our current `defloop` into a `defshards` that accepts arguments and create a new `defloop` that calls this new `defshards` as shown below.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))
    ```

    ```{.clojure .annotate linenums="1"}
    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))
    ```

    ```{.clojure .annotate linenums="1"}
    (Step random-coin-1)
    (Step random-coin-2)
    ```

Now, last but not least, we will convert our scoring logic into a ``defshards`` that accepts arguments.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
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
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)

    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))

    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score
      false >= .scored

      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit)

    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))

    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements))

      (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
      (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)

      (run-logic)
      (gravity-logic)
      (scoring)

      (Step idle-animation)
      (Step walking-animation)
      (Step coin-animation)

      (Step random-coin-1)
      (Step random-coin-2)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
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
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Now with all the pieces in place, we can add as many coins as we want.

In this tutorial we will stick to just four coins, but you can create as many as you want! 

Just remember to follow the same process and flow as the above to make sure that you don't miss out something.

In summary:

1. Create the necessary variables for your new coins.

2. Create a new `UI.Area` for each of your new coins.

3. Make sure your new coins fall by adding new `coin-gravity-logic`.

4. Make sure your coins loop and randomize by adding a new `random-coin-loop`.

5. Make sure you can earn points from them by adding a new `score-collision`.

## Recap

Phew that was a really long step!

Good job for making it through. 😁🎉

To recap:

- We made a coin fall by using the same logic to make our character move.

- We created a scoring system by making a collision system.

- We set up a system to create as many coins as we want by making our `defshards` accept arguments.

Phew! That was a mouthful but worth it!

In the next chapter, we will use the same logic to create falling hazards for Glod!

See you! 👋🏼😀
