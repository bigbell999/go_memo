package
------------
1. go는 c언어와 비슷하게 컴파일러는 main함수와 main package를 먼저 찾아 실행시킴.      
따라서 main package와 main 함수 없이는 컴파일 될 수 없음.      
        
2. Println이 대문자로 시작하는 이유     
다른 패키지로 함수를 export하기 위해서는 대문자로 시작해야함 소문자시작 함수는 private이므로 export불가능      
       
       
상수 변수
-----------
1.상수선언       

	const Bbell string "bbell"        

상수선언시 자료형 입력해야함.

2. 변수선언         

```
var name string = "bbell"            
name := "bbell"             
```

단, 축약형은 함수내에서만 사용가능.         
             
             
func
--------
1. 함수선언         

	func (para 자료형) 반환값 자료형 {}

2.  하나의 함수가 여러개의 반환값을 가질 수 있음.           
쓰지 않고 싶은 value가 있다면 _(ignored value)로 할당해주면 됨.            

3. 여러개의 인수 입력             

	func (para... string) string {}

여러개의 인수들이 array로 저장.            

4. naked return            

```
func (para string) para11 {
	para11 += para
	return
} 
```

함수 반환값에 반환될 변수를 선언가능.              
마지막에 return만 써주면 알아서 찾아줌.           

5. defer              

	defer fmt.Println("defer: 함수가 리턴한 뒤 실행되는 코드")
	
	
loop
----------
1.  range               

	index, value = range(array)
	
range는 for loop안에서만 사용할 수 있다.                            
          
	 

조건문
---------
1.  variable expression            
if-else/switch를 시작하면서 변수를 선언가능.              
이때 이 변수는 if-else문 안에서만 사용되는 지역변수.                


배열
--------
1. array 선언           

	array := [크기]자료형{} 

2. slice 선언             

	slice :=[]자료형{} //slice: 길이가 정해지지 않은 array           

3.  slice에 item추가         

	append(slice, item) //item을 추가한 slice return

4. append와 "..."                

	append(slice1, slice2...) //"..."을 추가함으로써 slice2에 있는 원소들을 siice1에 추가할 수 있음.
	
##map
1. map 선언         
map == 파이썬의 dict             

	map이름 := map[key의 자료형]value의 자료형{x: y}   

2. map 순회              
for loop의 range를 통해 순회가능.   

	key, value := range(map)   

3. map의 키값 찾기           

	i, ok := map["key"] //여기서 i는 value, ok는 boolean

4. map에서 원하는 키값찾기 메소드            

```
func (d Ditionary) Search(wanted string) (string, error) {
	value, found := d[wanted]
	if found {
		return value, nil
	}
	return "", errNoFound
}
```

5. delete           

delete(map, word) //word item삭제함. 반환값, 키값 필요없음.

6. 빈 map을 만들 때               

'''
emptyMap := map[string]string{}
//emptyMap := make(map[string]string
'''

이렇게 하지 않으면 map은 nil을 반환한다. -> map에 데이터 추가가 불가능함.  

7. 새로운 type 선언             

	type (이름) map[string]string
 
 
struct
-------
1. struct 정의             
struct c언어의 구조체와 같다           

```
type structName struct{   
	value1 dataForm,   
	value2 dataForm,   
}
``` 

2. struct 선언

```
변수이름 := structName{"bbell", "20"} 
//structName{name: "bbell", old: "20"} 선언가능 이것을 권장.
```


export
--------
1. 함수 export             
함수를 export하기 위해서는         
(1)함수이름 대문자 시작           
(2)함수 이름으로 시작하는 주석달아주기 (필수)          

2. struct export          
struct의 field들도 대문자로 작성해야 다른 파일에서 접근가능함.(public)         
struct의 요소들은 private으로 설정하고 struct를 만들 수 있는 함수를 public으로 설정하는 방법을 주로 사용.           
이때 함수는 포인터등의 object를 반환.          



메소드
------------
1. 메소드 정의          
 
	func (s structName) methodName(para) {} //접근하려는 struct의 첫글자를 소문자로 적는데, 이를 receiver라 한다.

2. 직접 참조        

	func (s *structName) methodName(para) {}

기본적으로 메소드는 object를 복사하여 사용.        
이때 object에 변경하고 싶은 것이 있을 때는 receiver 뒤 struct를 *struct로 고친다.            
메모리를 직접 참조 가능.             

3. String()메소드                   
String() 메소드는 python의 __str__과 역할이 같다.     

	return fmt.Sprint() 


error
-----------
1. error의 value             
error의 값은 error나 nil(NULL) 두가지       

2. errors.New()        

	error := errors.New("출력문")  //"출력문"을 출력하고 error값을 반환


3. go의 error 처리             
조건문으로 error를 직접 처리해야함.           

```
if (error 값) != nil {
log.Fatalln(error값) //Println호출하고 프로그램 종료 fmt.Println도 사용가능 단, 이경우 종료는 되지 않음.
}
```

보통 위 조건문을 checkErr()함수로 만들어 사용함.           

4. error를 변수로 저장하기.             
error를 변수로 저장가능 단, 이때 변수의 이름은 err~~               

5. error: panic           
panic : 컴파일러가 찾지 못하는 에러     


goroutine
------------
1. goroutine              
Goroutine()는 main함수와 독립적으로 실행되고 main함수가 종료되면 종료된다.                

2. time.Sleep()              

	time.Sleep(time.Second * n) //n초동안 sleep


3. channel 설정             

	c := make(chan 자료형)


4. blocking operation               
	<-c //채널로부터 메세지를 받아온다.              
메세지를을 받아 올때까지 main함수는 기다린다.            

5. channel                
한개의 채널이 여러개의 goroutine메세지를 받을 수 있음.               

6. channel과 loop                 
channel로 받아야 할 값이 많아질 때는 routine의 개수에 맞추어 loop                   

7. 함수에서 채널 방향 지정              

	func funcName(c chan<- result) {} //send only
  
8. strings.Join(strings.Fields(strings.TrimSpace(str)), " ")                   
  
```
strings.TrimSpace(str string) //str의 여백 모두 제거한 str 반환
string.Fields(str string) //str을 문자간 긴 공백에 따라 배열로 저장
strings.Join(s []string, sep string) //sep을 사이에 두고 배열의 원소를 합춰준다.
```

http package
--------
1. http.Get           

	resp, err := http.Get(url)
	

파일 처리
-----------
1. 파일 쓰기               

	Write(thing []string) error //slice를 버퍼에 저장하는 함수

2. Flush()메소드             

	file.Flush() //버퍼에 저장된 정보를 파일에 직접 쓰게해주는 메소드

3. Attachment 메소드               
	
	c.Attachment(fromFileName string, toFileName string) //fromFileName을 toFileName으로 바꾼첨부파일을 리턴



echo
--------
1. echo                
ctrl + c: echo서버 닫기
