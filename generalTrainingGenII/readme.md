# TrainingCTF2020 

Writeup dành cho các challenges trong thời gian Training của Gen II CLB ISP.

## 1. Sudoku  
[Source code](https://github.com/ispclub/generalTraining/blob/main/ctf/sudoku.cpp)

Đầu tiên mình chạy thử code và nhận được output sau:
```
	C:\ISP\source-code\TrainingGII>Sudoku.exe
	ispclub{d0_y0u_l1k3_5ud0ku?}
	(Y or N)? ====> Answer: N
	
	ispclub{y0u_d0_n0t_g3t_th3_fl4g}
  
	C:\ISP\source-code\TrainingGII>Sudoku.exe
	ispclub{d0_y0u_l1k3_5ud0ku?} 
	(Y or N)? ====> Answer: Y
	
	ispclub{l3t'5_try_4_puzzl3!}
							
	--------------------------
	| 7 0 0 | 6 0 0 | 2 0 3 |
	| 0 2 0 | 8 1 0 | 0 5 0 |
	| 0 0 5 | 0 9 2 | 0 0 0 |
	--------------------------
	| 2 0 0 | 0 7 0 | 5 0 8 |
	| 3 0 7 | 0 0 9 | 0 2 0 |
	| 0 9 0 | 0 4 0 | 0 0 7 |
	--------------------------
	| 1 0 8 | 0 3 7 | 0 0 2 |
	| 0 0 0 | 1 0 0 | 8 0 0 |
	| 6 0 2 | 0 0 0 | 9 3 0 |
	--------------------------
	Try again
```
Sau khi chạy cả 2 trường hợp "Y" và "N" mình nhận được tất cả 3 flag giả:  
```
  ispclub{d0_y0u_l1k3_5ud0ku?} 
  ispclub{l3t'5_try_4_puzzl3!}
  ispclub{y0u_d0_n0t_g3t_th3_fl4g}
```
Cùng check qua source code xem ta sẽ thu được gì:
- Dòng 20: *string sudoku="..."*  có 81 ký tự = 9x9. Có thể suy ra đây là ma trận sudoku.
- Dòng 37: *sudoku[++c]-97* dùng để chuyển các ký tự từ chữ về số theo mã ASCII.
- Dòng 42 - 45: Ta thấy string flag4 phụ thuộc vào các string flag1, flag2, flag3 và "sudoku". 
- Dòng 47: *if (flag4.size()==34) cout << flag4 << endl;*  nếu size flag4 bằng đúng với 34 ( = với size của flag4 dòng 19) thì sẽ in ra flag4. Vậy ta kết luận flag4 là flag của bài.  

Bây giờ ta phải đi giải bài sudoku này, thay các giá trị 0 bằng những số thích hợp. Ta chuyển dãy sudoku ấy về xâu chữ cái với *‘0’ = ’a’, ’1’ = ’b’, ’2’ = ’c’,...* Mình dùng 1 script `python` để làm việc này:  
```
C:\ISP\source-code\TrainingGII>python
Python 3.7.7
Type "help", "copyright", "credits" or "license" for more information.
>>> sudoku="haagaacadacaibaafaaafajcaaacaaahafaidahaajacaajaaeaaahbaiadhaacaaabaaiaagacaaajda"    <-- input string
>>> for i in sudoku:
...     print(ord(i)-97,end='')
...
700600203020810050005092000200070508307009020090040007108037002000100800602000930    <-- output string
>>>
``` 
Sau đó các bạn có thể tìm tool hoặc giải tay để luyện thêm trí não, còn ở đây mình tìm thấy 1 tool giúp giải sudoku khá tiện và mình đã chỉnh sửa lại để phù hợp giải chall này:  
[source code](https://pastebin.com/vmYLSBNP)  
```
C:\ISP\source-code\TrainingGII\>python Sudoku-solver.py
hbjgfecidecgibdhfjidfhjcebgcgedhbfjidihfgjbcefjbceidghbfijdhgecjedbcgihfghceifjdb   <-- output string
C:\ISP\source-code\TrainingGII\>
```
Sau đó thay chuỗi vừa tìm được vào source code challenge ban đầu và chạy lại:  
```
	C:\ISP\source-code\TrainingGII>Sudoku.exe
	ispclub{d0_y0u_l1k3_5ud0ku?}
	(Y or N)? ====> Answer: Y
	ispclub{l3t'5_try_4_puzzl3!}
	--------------------------
	| 7 1 9 | 6 5 4 | 2 8 3 |
	| 4 2 6 | 8 1 3 | 7 5 9 |
	| 8 3 5 | 7 9 2 | 4 1 6 |
	--------------------------
	| 2 6 4 | 3 7 1 | 5 9 8 |
	| 3 8 7 | 5 6 9 | 1 2 4 |
	| 5 9 1 | 2 4 8 | 3 6 7 |
	--------------------------
	| 1 5 8 | 9 3 7 | 6 4 2 |
	| 9 4 3 | 1 2 6 | 8 7 5 |
	| 6 7 2 | 4 8 5 | 9 3 1 |
	--------------------------
	ispclub{5ud0ku_puzzl3_15_n0t_345y}
```
Flag: `ispclub{5ud0ku_puzzl3_15_n0t_345y}`
