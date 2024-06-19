extends CharacterBody2D


const SPEED = 400.0
const JUMP_VELOCITY = -800.0
@onready var sprite_2d = $Sprite2D

# Get the gravity from the project settings to be synced with RigidBody nodes.
var gravity = ProjectSettings.get_setting("physics/2d/default_gravity")


func _physics_process(delta):
	if (velocity.x > 1 || velocity.x < -1):
		sprite_2d.animation = "run"
	else:
		sprite_2d.animation = "defult"
	
	# Add the gravity.
	if not is_on_floor():
		velocity.y += gravity * delta
		sprite_2d.animation = "jumping"
   
	# Handle Jump.
	if Input.is_action_just_pressed("up") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction = Input.get_axis("left", "right")
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0,20)

	move_and_slide()
	var isleft = velocity.x < 0
	sprite_2d.flip_h = isleft
func _on_body_entered(body): 
	if (body.name == "Area2D"):
		get_tree().reload_current_scene()







func _on_area_2d_body_entered(body): 
	if (body.name == "Area2D"):
		get_tree().reload_current_scene()
