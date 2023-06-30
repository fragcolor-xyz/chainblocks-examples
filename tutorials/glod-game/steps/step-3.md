# Step 3

##  Animating with Image Arrays - Overview

In the last step, we managed to have Glod's image change based on directional key input.

Now, let's breathe more life into him.

Instead of having static images, we can have him animated. To do this we will:

1. Use an image array to house the different frames in an animation.

2. Display a single frame in `UI.Area` using `UI.Image`.

3. Cycle through the image shown by going through the image array with `Step` to create a looping animation.

Without further ado, let's get to it!

## Step 3.1

Before we can create our animation, we have to download the images that will be used.

Create a folder within "GlodImages" and name it "Character_Idle". Within the "Character_Idle" folder, create another folder named "Idle_Left" and save the images there.

![Glod Idle Gif](assets/step3_1_Gif.gif){ style="display: block; margin: 0 auto; padding-bottom: 30px" }

- Download Glod's adorable idle animation frames [here](https://drive.google.com/drive/folders/1BJVK5WL5lBaL3coZXsDWB_YgOHW0f7fQ?usp=share_link).

## Step 3.2

Once you have the images downloaded, the next step is to have an image array to house these images. 

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ---------- Character Idle Array ---------- ;; (1)
    (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
    (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
    (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
    (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
    (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
    (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
    (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array  
    ```

    1. Remember to use `;;` comments to help segment and organize your code for better readability!

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}

    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .character-image
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state

      ;; ---------- Character Idle Array ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array)

    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state)))

    (defloop main-wire
      (Setup
       (initialize-character))

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
           :Contents
           (->
            .character-state
                (Match [0 (-> .character-image (UI.Image :Scale (float2 0.2)))
                        1 (-> .character-left (UI.Image :Scale (float2 0.2)))
                        2 (-> .character-right (UI.Image :Scale (float2 0.2)))
                        3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
                        :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

??? ">>"
    `>>` is an alias for the shard `Push`. An alias is a short form of a shard to make coding much easier. Think of it like typing "lol" instead of "laughing out loud". The `Push` shard is used to add an element to the back of an array.

## Step 3.3

Now that we have an array of images, we can take an element from this array and draw that specific image in the window. To do this:

1. Create a variable called `.idle-image-index`. This will determine the index of the image array that will be used.

2. Use the `Take` shard to retrieve the image located in the specified index of the image array.

3. Tell `UI.Area` to draw the image.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    0 >= .idle-image-index ;; (1)
    ```

    1. Added to line 22, within `initialize-character`.

    ```{.clojure .annotate linenums="1"}
    .idle-left-image-array (Take .idle-image-index) ;; (1)
    ```

    1. `.character-image` is renamed to `.idle-left-image-array` to better reflect its idling state at line 68.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}

    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .character-image
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state

      ;; ---------- Character Idle Array ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array
      
      0 >= .idle-image-index)

    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state)))

    (defloop main-wire
      (Setup
       (initialize-character))

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
           :Contents
           (->
            .character-state
                (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                        1 (-> .character-left (UI.Image :Scale (float2 0.2)))
                        2 (-> .character-right (UI.Image :Scale (float2 0.2)))
                        3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
                        :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 3.4

We're almost there!

To have this image animated, we need to dynamically increase the value of `.idle-image-index` and make it loop. To do this, we will first create a Wire that increases `.idle_image_index.`

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (defloop idle-animation ;; (1)
      .idle-image-index (Math.Add 1)
      > .idle-image-index)
    ```

    1. Use `defloop` to create a looping Wire that will add 1 to `.idle-image-index` each time it is called. Added to lines 24 - 26.

!!! note "Updating Variables"
  Whenever you make changes to a variable, remember to reassign it to the variable again at the end if you plan on using it in another segment of the code.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}

    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .character-image
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state

      ;; ---------- Character Idle Array ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array
      
      0 >= .idle-image-index)

    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index)
    
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state)))

    (defloop main-wire
      (Setup
       (initialize-character))

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
           :Contents
           (->
            .character-state
                (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                        1 (-> .character-left (UI.Image :Scale (float2 0.2)))
                        2 (-> .character-right (UI.Image :Scale (float2 0.2)))
                        3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
                        :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Before we can call this Wire, we have to create some variables to help us loop it properly.

If we were to try to use this Wire as it is, `.idle-image-index` will increase in value indefinitely.

To prevent this from happening, we will create some variables and a conditional statement to control how `.idle-image-index` increases.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (Count .idle-left-image-array) = .idle-image-index-max ;; (1)
    0.08 = .idle-animation-speed ;; (2)
    ```
  
    1. `.idle-image-index-max` will be used to ensure that `.idle-image-index `does not exceed this value. Added to line 23.
    2. `idle-animation-speed` ensures how fast our idle animation will play. Added to line 24.

    ```{.clojure .annotate linenums="1"}
    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation ;; (3)
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When ;; (1)
       :Predicate (IsMoreEqual .idle-image-index-max) 
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed)) ;; (2)
    ```

    1. The `When` conditional statement ensures that `.idle-animation-index` will not exceed `.idle-image-index-max`. This ensures that we will not try to draw an image that is beyond what the array has as it will crash the program.
    
    2. `Pause` is used as this `defloop` will eventually execute `Step`. This pause will cause the `defloop` to be called at the speed indicated by `.idle-animation-speed`.

    3. Added to lines 28 - 34.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}

    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .character-image
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state

      ;; ---------- Character Idle Array ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array
      
      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed)

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))
    
    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state)))

    (defloop main-wire
      (Setup
       (initialize-character))

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
           :Contents
           (->
            .character-state
                (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                        1 (-> .character-left (UI.Image :Scale (float2 0.2)))
                        2 (-> .character-right (UI.Image :Scale (float2 0.2)))
                        3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
                        :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Now we just have to call this Wire in our main Wire with `Step`. 

??? "Step"
    `Step` schedules and runs another Wire on the Wire calling `Step` itself.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (Step idle-animation) ;; (1)
    ```

    1. `Step` allows us to run a code in parallel to the main Wire. This line is added to line 62.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}

    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .character-image
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state

      ;; ---------- Character Idle Array ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array
      
      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed)

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When
       :Predicate (IsMoreEqual .idle-image-index-max)
       :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))
    
    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state)))

    (defloop main-wire
      (Setup
       (initialize-character))

      (Step idle-animation)

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
           :Contents
           (->
            .character-state
            (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                    1 (-> .character-left (UI.Image :Scale (float2 0.2)))
                    2 (-> .character-right (UI.Image :Scale (float2 0.2)))
                    3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Phew, that was a pretty long step! 😪 But it was worth it! Try running the code now. Tadah! You now have an animated idle image.

## Step 3.5

Before we proceed to the next chapter, let's give Glod more animations.

We will be:

1. Using the same logic to give Glod his walk animation.

2. Creating the logic to ensure that he faces the correct direction when the corresponding buttons are pressed. 

Create the appropriate folders and save the images accordingly:

- Download Glod's "Walking Right" animation [here](https://drive.google.com/drive/folders/1AovccGE5vE5D9fAgionRLbynfaichjWd?usp=share_link).

- Download Glod's "Walking Left" animation [here](https://drive.google.com/drive/folders/1_5h5ozXQ-DiOgyxZZw3K7cGoUw2NFFGG?usp=share_link).

- Download Glod's "Idle Right" animation [here](https://drive.google.com/drive/folders/1i7YxKPOFN9fVMmYNS5rEOCImsDuTAEhr?usp=share_link).

??? "Jumping Frame"
    There is no animation for jumping. Instead, it retains a single image while jumping.
    
    2D characters feel more responsive when they transit immediately between jumping and walking/idling. This is achieved by using only a single frame.

Similar to how we created an image array for the idle animation when Glod is facing left, we will now create one for when Glod is facing right.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array ;; (1)
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

    ```
    
    1. Added to line 22.
  
  Create image arrays for when Glod is walking left and for when Glod is walking right. Since the walking animation is different from the idle animation, we will be making new variables for:
  
  - Animation speed
  
  - Image index
  
  - Max image index

    ```{.clojure .annotate linenums="1"}
      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array ;; (1)
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

    ```

    1. Added to line 35.

`.character-left`, `.character-right` are updated to display an image from their respective arrays.

    ```{.clojure .annotate linenums="1"}

    .walking-left-image-array (Take .walking-image-index) ;; (1)
    .walking-right-image-array (Take .walking-image-index)
    ```

    1. Update lines 115 - 116.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .idle-left-image-array
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state

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
    
    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state)))

    (defloop main-wire
      (Setup
       (initialize-character))

      (Step idle-animation)

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
           :Contents
           (->
            .character-state
            (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Create a Wire for the walking animation and `Step` it.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
     ;; -------- Walking Animation Loop --------
    (defloop walking-animation ;; (1)
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When
       :Predicate (IsMoreEqual .walking-image-index-max) 
       :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))
    ```

    1. Added to line 67.

    ```{.clojure .annotate linenums="1"}
      (Step walking-animation) ;; (1)
    ```

    1. Added to line 104.

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards load-texture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .idle-left-image-array
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state

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
        1 > .character-state))

      (Inputs.KeyDown
       :Key "right"
       :Action
       (->
        (Msg "right")
        2 > .character-state))

      (Inputs.KeyDown
       :Key "up"
       :Action
       (->
        (Msg "up")
        3 > .character-state)))

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
           :Contents
           (->
            .character-state
            (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                    1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                    3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
                   :Passthrough false)))))

        (GFX.Render :Steps .render-steps)

        (button-inputs))))

    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 3.6

If you tried running the program, you might notice some problems.

Namely:

1. Glod does not return to his idle animation.
2. Glod's jumping animation only faces right.

Let's fix these problems.

Why don't we start with the easier one first? Let's get Glod to return to his idle animation when we are not pressing any buttons.

To do this we will:

1. Use `Inputs.KeyUp`.

2. Create a variable that tracks which direction Glod is facing towards.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    0 >= .character-direction ;; 0 = facing left, 1 = facing right ;; (1)
    ```

    1. Added to line 12 under `initialize-character`.

    ;; ------- Button Inputs ----------
    (defshards button-inputs [] ;; (1)
      (Inputs.KeyDown
       :Key "left"
       :Action
       (->
        (Msg "left")
        1 > .character-state
        0 > .character-direction))

      (Inputs.KeyDown ;; (3)
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

      (Inputs.KeyUp ;; (2)
       :Key "left"
       :Action 
       (->
        0 > .character-state))

      (Inputs.KeyUp
       :Key "right"
       :Action
       (->
        0 > .character-state)))
    ```

    1. Added to line 78.
    
    2. When the left and right buttons are released, `.character-state` will revert back to 0.
    
    3. When the left and right buttons are pressed `.character-direction` will be 0 and 1 respectively.

Lastly, we will add another `Match` nested within `Match [0(->)]` so that Glod displays the correct image based on his direction.

    ```{.clojure .annotate linenums="1"}
      (Match [0 (-> .character-direction
                    (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                            1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                            :Passthrough false))
              1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
              2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
              3 (-> .character-jumping (UI.Image :Scale (float2 0.2)))]
              :Passthrough false)
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1.png") = .character-image
      (LoadTexture "GlodImages/Character1_Left.png") = .character-left
      (LoadTexture "GlodImages/Character1_Right.png") = .character-right
      (LoadTexture "GlodImages/Character1_Jumping.png") = .character-jumping

      0 >= .character-state
      0 >= .character-direction

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

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")
                1 > .character-state
                0 > .character-direction))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")
                2 > .character-state
                1 > .character-direction))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state)))

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character))

      (Step idle-animation)
      (Step walking-animation)

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
            (UI.Area :Position (float2 0 0)
                      :Anchor Anchor.Center
                      :Contents
                      (->
                      .character-state
                      (Match [0 (-> .character-direction
                                    (Match [0 (-> LoadTexture .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                            1 (-> LoadTexture .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                            :Passthrough false))
                              1 (-> LoadTexture .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                              2 (-> LoadTexture .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                              3 (-> LoadTexture .character-jumping (UI.Image :Scale (float2 0.2)))]
                              :Passthrough false)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

When you run the programme, Glod will  return to his idle animation whenever you stop pressing the left, right or up buttons and he should be facing the correct direction. Now we have to do the same thing for our jump animation

- Download Jumping Image [here](https://drive.google.com/drive/folders/1l5y2PoZARAukfsNz_qeQXQblrrpnwy6i?usp=share_link).

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; -------------- Character Jumping  ----------
    (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
    (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right
    ```

    > Add in a jumping right image and rename our original **.character-jumping** to **.character-jumping-left**. Code added to lines 6-8. Remove the other images that are now not being used anymore.

    ```{.clojure .annotate linenums="1"}
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
            :Passthrough false)
    ```

    > Then add another **Match** to our original **Match** to show the corresponding jump image depending on Glod's direction. Code added to lines 144-148.

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
           :Position (float2 0 0)
           :Anchor Anchor.Center
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
Try running the code now! Your adorable Glod should be fully animated and will now face the correct directions.

Congratulations on making your first animated character in Shards.

To recap, in this step, we used arrays of images to animate Glod. Next, we will break the chains that bind Glod down and give him the power to move.

See you in the next chapter! 😄
