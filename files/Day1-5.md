# 첫 5주차 동안은 게임의 기획을 해보고 틀을 구현하는 작업을 하였습니다.


## 기획 단계

[기획서 링크](https://nbviewer.org/github/ForMeAccount/GameProject_002.github.io/blob/main/reference/1588026_%EC%9C%A0%EC%8A%B9%ED%9A%A8_%ED%8F%AC%ED%8A%B8%ED%8F%B4%EB%A6%AC%EC%98%A4_%EC%B6%94%EA%B0%80.pdf)

## Player 구현

    public float moveSpeed;
    public float turnSpeed = 360.0f;
    public bool isMoveState = false;

    public Camera player_cam;

    CharacterController _cc;
    Vector3 destination;


    private Animator playerAnimator; 
    void Awake()
    {
        _cc = GetComponent<CharacterController>();
    }

    private void Start()
    {
        playerAnimator = GetComponent<Animator>();
    }

    void Update()
    {


        if (Input.GetMouseButton(1))
        {
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            RaycastHit hitInfo;

            if (Physics.Raycast(ray, out hitInfo, 100f))
            {
                destination = hitInfo.point;
                isMoveState = true;
            }
        }

        if (isMoveState)
        {

            Vector3 moveDir = destination - transform.position;
            Vector3 dirXZ = new Vector3(moveDir.x, 0, moveDir.z);
            Vector3 targetPos = transform.position + dirXZ;

            Vector3 framePos = Vector3.MoveTowards(transform.position, targetPos, moveSpeed * Time.deltaTime);
            Vector3 frameDir = framePos -  transform.position;

            _cc.Move(frameDir + Physics.gravity);

            transform.rotation = Quaternion.RotateTowards(transform.rotation, Quaternion.LookRotation(frameDir), turnSpeed * Time.deltaTime);


            if (framePos == targetPos)
            {
                isMoveState = false;
            }
            playerAnimator.SetFloat("Move", 1);
        } else
        {
            playerAnimator.SetFloat("Move", 0);
        }
    }
