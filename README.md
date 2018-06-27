# 3D-Controller
Controller Script fro Dodgeball Game in Unity.


using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Controller3D : MonoBehaviour {

	// Use this for initialization
	public GameObject ball;
	private Transform transform;
	private Rigidbody body;
	private float sizeX;
	private float sizeY;
	private Vector3 move;
	public float xSpeed = 5f;
	public float zSpeed = 5f; 
	public float jumpSpeed = 10f;
	private Vector3 velocity;
	private float angle;

	private bool canDodge;

	public float dodgeSpeed = 10f;

	public float grabRadius = 5f;
	private bool ballGrabbed = false;


	void Start () {
		// controller = gameObject.GetComponent<CharacterController2D> ();
		body = gameObject.GetComponent<Rigidbody>();
		transform = gameObject.transform;
		sizeX = transform.localScale.x;
		sizeY = transform.localScale.y;
	}
	
	// Update is called once per frame
	void Update () {
		MoveInput ();
		GrabInput ();
	}

	void MoveInput(){
		// handle XY Input
		move.x = Input.GetAxis("Horizontal");
		move.z = Input.GetAxis("Vertical");

		//TODO increase in Z means increase in Y as well

		velocity.x = (move.x * xSpeed)/Time.deltaTime;
		velocity.z = (move.z * zSpeed)/Time.deltaTime;
		body.velocity = velocity;
		angle = Mathf.Atan2(velocity.y, velocity.x) * Mathf.Rad2Deg; 
		body.transform.eulerAngles = new Vector3 (0f, 0f,angle); // TODO incorporate Z

		//handle Dodge/Jump Input
		if (Input.GetButton ("Jump")) {
			print ("dodge");

			// TODO vary force on Input. I.e implement an actual jump

			body.AddForce (new Vector3 (velocity.x * dodgeSpeed,0f,velocity.y * dodgeSpeed) );
		}
		print ("move = " + move);

	}

	void GrabInput(){
		if (Input.GetMouseButton (0)) {
			if (transform.position.x + grabRadius > ball.transform.position.x &&
			    transform.position.x - grabRadius < ball.transform.position.x) {
				if (transform.position.y + grabRadius > ball.transform.position.y &&
				    transform.position.y - grabRadius < ball.transform.position.y) {

					// TODO Z axis 	

					ballGrabbed = true;
				}
			}
		} else {
			ballGrabbed = false;
		}
		if (ballGrabbed) {
			ball.transform.position = new Vector3 (transform.position.x+ 3,transform.position.y+ 3 , 0f);
		}
	}

	void OnCollisionEnter3D(Collision collision) {
		
		if (collision.gameObject.tag == "Ball") {
			print (" Ball collision! ");
		}
	}
		
}
