---
title: "2020년 11월 21일"
excerpt: "JDBC 과제 : 단군마켓 "
search: true
categories: 
  - Parctice
  - Academy
tags: 
  - JAVA
  - JDBC
  - TIL
  - 단군마켓
  - 보령, 혜윤, 만희 팀플
toc: true
---

## JDBC
### 단군마켓 Project
> 중고거래를 하는 마켓.<br/>
  {: .notice--success}


```java
//JurrRun.java
package manat.jurr.run;

import manat.jurr.view.JurrView;

public class JurrRun {
	public static void main(String[] args) {

		new JurrView().displayMain();
		
	}

}

```
```java
//JurrViewInterface.java
package manat.jurr.view;

public interface JurrViewInterface {
	public abstract void displayMain();
	//로그인, 회원가입 method
	public abstract void login();
	public abstract void signUp();
    public abstract void registerProduct();
	
	
	public abstract void itemMenu();
	//상품 조회 서브메뉴 method
	public abstract void searchAllItem();
	public abstract void searchKeyword();
	public abstract void purchaseItem();
	
	
	public abstract void myMenu();
	//마이페이지 서브메뉴 method
	public abstract void selectMyBoard();
	public abstract void updateMyBoard();
	public abstract void deleteMyBoard();
	public abstract int checkMyBoard();
	public abstract void myDanKunLV();
	public abstract void updateMyInfo();
	public abstract void updateSecessionMember();

}


```

```java
//JurrView.java
package manat.jurr.view;

import static manat.jurr.common.JurrTemplate.*;

import java.util.HashMap;
import java.util.InputMismatchException;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

import manat.jurr.model.service.MemberService;
import manat.jurr.model.service.ProductService;
import manat.jurr.model.vo.Member;
import manat.jurr.model.vo.Product;
import manat.jurr.model.vo.VProduct;

public class JurrView implements JurrViewInterface {


	private Scanner sc = new Scanner(System.in);
	private MemberService mService = new MemberService();
	private ProductService pService = new ProductService();
	public static Member loginMember = null;



	/**작성자 김만희
	 * 메인 메뉴
	 */
	@Override
	public void displayMain() {

		int sel = 0;

		do {
			try {
				if(loginMember == null) { 
					System.out.println("◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆");
	                  System.out.println("◆◇◆◇ JURR PROJECT◆◇◆◇");
	                  System.out.println("◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆");
	                  System.out.println("▷▷▷ 1.  로그인  　◁◁◁◁◁");
	                  System.out.println("▶▶▶ 2.  회원가입  ◀◀◀◀◀");
	                  System.out.println("▷▷▷ 0.　프로그램 종료◁◁◁◁");
	                  System.out.println("◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇");

					System.out.print("메뉴 선택 >> ");
					sel = sc.nextInt();
					sc.nextLine();

					System.out.println();
					switch(sel) {
					case 1 : login(); break;
					case 2 : signUp(); break;
					case 0 : System.out.println("프로그램 종료~"); break;
					default : System.out.println("1, 2, 0 만 입력해~!");
					}
				}else {
					System.out.println();
					System.out.println("ºººººººººººººººººººººººººººººº");
					System.out.println("[ 메인 메뉴 ]");
					System.out.println("1. 상품 구매");
					System.out.println("2. 상품 등록 ");
					System.out.println("3. My Menu");
					System.out.println("9. 로그아웃");
					System.out.println("0. 프로그램 종료");
					System.out.println("문의 전화 : 010 - 8954 -6041 ");
					System.out.println("주소 : 남대문로 120 2F,3F 쥬르텍");
					System.out.println("ººººººººººººººººººººººººººººº");

					System.out.print("메뉴 선택 : ");
					sel = sc.nextInt();
					sc.nextLine();

					System.out.println();
					switch(sel) {
					case 1 : itemMenu(); break;
					case 2 : registerProduct(); break;
					case 3 : myMenu(); break;
					case 9 : loginMember = null;
							System.out.println("로그아웃 되었습니다");
							break;
					case 0 : System.out.println("프로그램 종료"); break;
					default : System.out.println("잘못 입력하셨습니다");
					}
				}
			}catch(InputMismatchException e) {
				System.out.println("숫자만 입력해주세요");
				sel = -1;
				sc.nextLine();
			}
		}while(sel != 0);


	}





	/** 작성자 김만희
	 * 회원가입용 View
	 */
	@Override
	public void signUp() {
	      System.out.println("[ 회원 가입]");

	      System.out.print("아이디 : ");
	      String memId = sc.nextLine();
	      System.out.print("비밀번호 : ");
	      String memPw = sc.nextLine();
	      System.out.print("이름 : ");
	      String memNm = sc.nextLine();
	      System.out.print("전화번호 : ");
	      String phone = sc.nextLine();
	      System.out.print("주소 : ");
	      String address = sc.nextLine();

	      Member newMember = new Member(memId, memPw, memNm, phone, address);
	      
	      String random = "";
	      
	      int i = 0;   
	      while(i<=5) {
	         char num = (char)((Math.random()*74) +49) ;
	         if(num > '9' && num < 'A' || num > 'Z' && num < 'a') {
	            continue;
	         }else {
	            random += num;
	            i++;
	         }
	      }
	      
	      System.out.println("자동회원가입 방지시스템" );
	      System.out.println("["+ random + "] 을 입력하세요." );
	      System.out.print("입력 : ");
	      String input = sc.nextLine();

	      if(input.equals(random)) {
	      
	      try {
	         int result = mService.signUp(newMember);

	         if(result > 0 ) {
	            System.out.println("축하합니다!" + memNm + "님! 단군마켓의 회원이 되셨습니다");
	         }else {
	            System.out.println("회원가입하기 싫으신가요?");
	         }

	      } catch (Exception e) {
	         System.out.println("회원가입 과정에서 오류 발생");
	         e.printStackTrace();
	      }
	         
	      }else {
	         System.out.println("자동 회원가입 방지!!!");
	      }
	         
	   }


	/** 작성자 김만희
	 * 로그인용 View
	 */
	@Override
	public void login() {
		System.out.println("[ 로그인  ]");
		System.out.print("아이디 : ");
		String memId = sc.nextLine();

		System.out.print("비밀번호 : ");
		String memPw = sc.next();

		Member member = new Member();
		member.setMemId(memId);
		member.setMemPw(memPw);

		try {
			loginMember = mService.loginMember(member);

			if(loginMember != null) {
				System.out.println(loginMember.getMemNm() + "님 환영합니다.");
			}else {
				System.out.println("아이디 또는 비밀번호가 일치하지 않거나 , 탈퇴한 회원입니다.");
			}

		} catch (Exception e) {
			System.out.println("로그인 과정에서 오류가 발생했습니다.");
			e.printStackTrace();
		}

	}

	/** 작성자 김만희
	 * 상품 등록 View
	 */
	@Override
	public void registerProduct() {
		System.out.println("[ 상품 등록 ]");

		System.out.print("카테고리 ( 의류  / 생활용품  / 식품  / 기타 ) :");
		String kind = sc.nextLine();

		System.out.print("상품 이름 : ");
		String itemNm = sc.nextLine();

		System.out.print("상품 가격 : ");
		int price = sc.nextInt();
		sc.nextLine();

		System.out.print("위치 : ");
		String address = sc.nextLine();

		StringBuffer sb = new StringBuffer();

		String str = null;

		System.out.println("상품 설명! '끗!' 입력시 작성완료");

		while(true) {
			str = sc.nextLine();

			if(str.equals("끗!")) break;

			sb.append(str + "\n");
		}
		// 종류, 상품명, 가격, 주소, 상품 설명, 회원번호, 종류
		try {
			Product product = new Product(itemNm, 
					price,
					kind,
					address,
					sb.toString(),
					JurrView.loginMember.getMemNo()
					);

			int result = pService.registerProduct(product);

			if(result > 0 ) System.out.println("상품이 등록되었습니다.");
			else 			System.out.println("상품등록에 실패했습니다.");

		} catch (Exception e) {
			System.out.println("상품 등록중 오류 발생");
			e.printStackTrace();
		}

	}




	/** 상품 검색/구매 서브메뉴
	 *@author 박혜윤
	 */
	@Override
	public void itemMenu() {
		int sel = 0;

		do {
			try {
				System.out.println("=============================================");
				System.out.println("[상품 검색 및 구매]");
				System.out.println("1. 전체 상품 조회");
				System.out.println("2. 키워드로 판매글 상세 조회");
				System.out.println("3. 구매하기");
				System.out.println("0. 메인메뉴로 돌아가기");
				System.out.println("=============================================");
				System.out.print("메뉴 선택 : ");
				sel = sc.nextInt();
				sc.nextLine();
				System.out.println();

				switch(sel) {
				case 1 : searchAllItem(); break;
				case 2 : searchKeyword(); break;
				case 3 : purchaseItem(); break;
				case 0 : displayMain(); break;
				default : System.out.println("잘못 입력하셨습니다. 다시 선택해주세요.");
				}
			} catch(InputMismatchException e) {
				System.out.println("숫자만 입력하세요.");
				sel = -1;
				sc.nextLine();
			} catch(Exception e) {
				System.out.println("오류가 발생하였습니다.\n다시 입력하세요.");
				sel = -1;
				e.printStackTrace();
			}
		} while (sel != 0);

	}

	/** 모든 판매글 간단 조회 view
	 *@author 박혜윤
	 */
	@Override
	public void searchAllItem() {
		System.out.println("[상품 목록]");
		try {
			List<VProduct> list = pService.selectAllItem();

			if(!list.isEmpty()) {
				System.out.printf(" %s | %s | %s | %s | %s\n",
						"No.", "카테고리", "상품명", "가격", "판매자");
				for(VProduct item : list) {
					System.out.printf(" %d | %s | %s | %d | %s\n",
							item.getItemNo(), item.getKind(), 
							item.getItemNm(), item.getPrice(), 
							item.getMemId());
				}
			}else {
				System.out.println("판매글이 없습니다. 첫 판매글을 남겨보세요!");
			}
		} catch(Exception e) {
			e.printStackTrace();
		}

	}

	/** 검색으로 판매글 상세 조회 view
	 *@author 박혜윤
	 */
	@Override
	public void searchKeyword() {
		System.out.println("=============================================");
		System.out.println("[검색으로 상품 조회]");
		System.out.println("1. 상품명 조회");
		System.out.println("2. 카테고리 조회 (생활용품, 식품, 의류, 기타)");
		System.out.println("3. 가격으로 조회");
		System.out.println("4. 위치로 조회");
		System.out.println("0. 취소");
		System.out.println("=============================================");
		System.out.print("선택: ");
		int sel = sc.nextInt();
		sc.nextLine();

		if(sel == 0) {
			System.out.println("검색 취소하셨습니다."); 
		} else if(sel >= 1 && sel <= 4) {
			System.out.print("검색어 입력: ");
			String keyword = sc.nextLine();

			Map<String, Object> map = new HashMap<>();
			map.put("sel", sel);
			map.put("keyword", keyword);

			try {
				List<VProduct> list = pService.searchKeyword(map);


				System.out.printf(" %s | %s | %s | %s | %s | %s\n",
						"No.", "카테고리", "상품명", "판매자", "가격", "지역");
				System.out.println("=============================================");

				for(VProduct product : list) {
					System.out.printf(" %d | %s | %s | %s | %d | %s\n", 
							product.getItemNo(), product.getKind(), product.getItemNm(),
							product.getMemId(), product.getPrice(), product.getAddress());
					System.out.println("=============================================");
					System.out.println(product.getContent());
					System.out.println();
					System.out.println("=============================================");

				}	

			} catch (Exception e) {
				System.out.println("게시글 검색 중 오류 발생");
				e.printStackTrace();
			}
		} else {
			System.out.println("잘못 입력하셨습니다.");
		}
	}


	/** 상품 번호로 상품 구매 후 포인트 적립 view
	 * @author 박혜윤
	 */
	@Override
	public void purchaseItem() {
		System.out.println("[상품 구매]");
		System.out.print("구매하실 상품 번호를 입력하세요: ");
		int num = sc.nextInt();
		sc.nextLine();

		try {
			VProduct item = pService.searchItem(num);
			System.out.printf(" %s | %s | %s | %s | %s\n",
					"No.", "카테고리", "상품명", "가격", "판매자");
			System.out.printf(" %d | %s | %s | %d | %s\n",
					item.getItemNo(), item.getKind(), 
					item.getItemNm(), item.getPrice(), 
					item.getMemId());
			System.out.println("주문하실 상품이 맞습니까? (Y/N): ");
			char sel = sc.nextLine().toUpperCase().charAt(0);

			if(sel == 'Y') {
				int result = pService.purchaseItem(num);

				if(result>0) {
					System.out.println(num + "번 상품이 구매되었습니다.");
				} else {
					System.out.println("구매 실패하였습니다. 고객센터 1555-5555");
				}
			} else if(sel == 'N') {
				System.out.println("구매를 취소하셨습니다.");
			} else {
				System.out.println("잘못 입력하셨습니다.");
			}

		} catch(Exception e) {
			System.out.println("오류가 발생하였습니다. 다시 시도해주세요.");
			e.printStackTrace();
		}

	}	




	/** 작성자 강보령
	 * My Menu View
	 */
	public void myMenu() {
		int sel =0;

		do {
			try {
				if(loginMember == null) return;
				System.out.println("********************");
				System.out.println("***~~~~MY PAGE~~~~***");
				System.out.println("* 1. 내가 쓴 글 조회하기 *");
				System.out.println("* 2. 내가 쓴 글 수정하기 *");
				System.out.println("* 3. 내가 쓴 글 삭제하기 *");
				System.out.println("* 4. 나의 단군_LV 조회 *");
				System.out.println("* 5. 회원 정보 수정        *");
				System.out.println("* 6. 회원 탈퇴              *");
				System.out.println("* 0. 메인메뉴로..    *");
				System.out.println("*******************");
				System.out.print("*메뉴 선택>>>");
				sel = sc.nextInt();
				sc.nextLine();

				switch(sel) {
				case 1: selectMyBoard(); break;
				case 2: updateMyBoard(); break;
				case 3: deleteMyBoard(); break;
				case 4: myDanKunLV(); break;
				case 5: updateMyInfo(); break;
				case 6: updateSecessionMember();break;
				case 0: displayMain(); break;
				default: break;

				}
			}catch(InputMismatchException e) {
				System.out.println("잘못 입력하셨습니다. 다시 입력해주세요");
				sel = -1;
				sc.nextLine();
			}catch(Exception e) {
				e.printStackTrace();
			}

		}while(sel !=0);


	}




	/** 작성자 강보령
	 *  내가 쓴 글 전체 조회  View
	 */
	@Override
	public void selectMyBoard() {
		System.out.println("*[내가 쓴 글 전체 목록]*");
		try {
			String memId = loginMember.getMemId();

			List<VProduct> list = mService.selectMyBoard(memId);

			if(list.isEmpty()) {
				System.out.println("작성된 게시글이 없습니다.");
			}
			for(VProduct p : list) {
				System.out.printf("**%s | **%s |     **%s        |   **%s     |   **%s\n", "게시글 번호", "회원 아이디", "품명", "가격", "종류");
				System.out.printf("     %d    |   %s    |   %s    |   %d원   |  %s\n", p.getItemNo(), p.getMemId(), p.getItemNm(), p.getPrice(), p.getKind());
				System.out.println();
			}

		}catch(Exception e) {
			System.out.println("게시글 조회 시 오류가 발생했습니다.");
			e.printStackTrace();
		}

	}




	/**작성자 강보령
	 * 내가 쓴 글 수정 View
	 */
	@Override
	public void updateMyBoard() {
		System.out.println("**게시글 수정**");	
		int itemNo = checkMyBoard();

		if(itemNo>0) {
			System.out.println("~~게시글 수정~~");
			System.out.print("제품명 : ");
			String name = sc.nextLine();

			System.out.print("가격 : ");
			int price = sc.nextInt();
			sc.nextLine();

			System.out.print("내용 (exit를 입력하면 종료됩니다.): ");
			StringBuffer sb = new StringBuffer();
			String str = "";

			while(true) {
				str = sc.nextLine();
				if(str.equals("exit")) {
					break;
				}
				sb.append(str+"\n");
			}

			try {

				Product update = new Product(name, price, sb.toString(), itemNo);

				int result = mService.updateMyBoard(update); 

				if(result >0) {
					System.out.println("수정되었습니다.");
					System.out.println();

				}else {
					System.out.println("수정에 실패했습니다.");
				}
			}catch(Exception e) {
				System.out.println("게시글 수정 과정에서 오류가 발생했습니다.");
				e.printStackTrace();
			}
		}
	}



	/** 작성자 강보령
	 *  내가 쓴 글 삭제 View
	 */
	@Override
	public void deleteMyBoard() {
		System.out.println("**게시글 삭제**");
		int itemNo = checkMyBoard();

		if(itemNo>0) {
			try {
				System.out.print("정말 삭제하시겠습니까?(Y/N) : ");
				char input = sc.nextLine().toUpperCase().charAt(0);
				if(input == 'Y') {

					int result = mService.deleteMyBoard(itemNo); 

					if(result >0) {
						System.out.println("삭제되었습니다.");
						System.out.println();
					}else {
						System.out.println("삭제에 실패했습니다.");
						System.out.println();
					}

				}else {
					System.out.println("게시글 삭제를 취소했습니다.");
					System.out.println();
				}

			}catch(Exception e) {
				System.out.println("게시글 삭제 과정에서 오류가 발생했습니다.");
				e.printStackTrace();
			}
		}

	}


	/** 작성자 강보령 
	 * 게시글 수정, 삭제 할때 본인글인지 확인용 View
	 * @return result
	 */
	@Override
	public int checkMyBoard() {
		// 게시글 번호 입력
		System.out.print("**게시글 번호 입력 : ");
		int itemNo = sc.nextInt();
		sc.nextLine();

		int result = 0; // 글이 존재하는지에 대한 판별 결과를 저장할 변수

		try {
			Product product = new Product();
			product.setItemNo(itemNo);
			product.setMemNo(loginMember.getMemNo()); 

			result = mService.checkMyBoard(product);

			if(result>0) { // 입력한 번호의 글이 로그인한 회원의 글인 경우
				System.out.println("*~~ 확인 완료 ~~*");
				result = itemNo;

			} else {
				System.out.println("자신의 글이 아니거나, 존재하지 않는 글번호 입니다.");
			}

		}catch (Exception e) {
			System.out.println("게시글 확인 과정에서 오류가 발생했습니다.");
			e.printStackTrace();
		}
		return result;
	}






	/** 작성자 강보령
	 * 나의 단군 레벨 조회 View
	 */
	@Override
	public void myDanKunLV() {
		System.out.println("~**~나의 단군 레벨~**~");
		try {
			
			int memNo = loginMember.getMemNo();
			
			
			Member member = mService.myDanKunLV(memNo);
			System.out.println("테스트 3:");
			//loginMember.setDankunLv(member.getDankunLv());	
			System.out.println(member.getMemNm()+"회원님은 LV."+ member.getDankunLv() +"(POINT:"+member.getPoint() +"점) 입니다."); 

		}catch(Exception e){
			System.out.println("레벨 조회과정에서 오류가 발생했습니다.");
			e.printStackTrace();
		}

	}





	/** 작성자 강보령
	 * 회원정보 수정 View
	 */
	@Override
	public void updateMyInfo() {
		System.out.println("**~ 내 정보 수정하기 ~**");

		System.out.println("이름 : ");
		String name = sc.nextLine();

		System.out.println("전화번호 : ");
		String phone =sc.nextLine();

		System.out.println("주소 : ");
		String address = sc.nextLine();


		Member member = new Member( loginMember.getMemNo(), name, phone, address);

		try {
			int result = mService.updateMyInfo(member);

			if(result>0) {
				System.out.println("정보가 정상적으로 수정되었습니다.");

				loginMember.setMemNm(name);
				loginMember.setPhone(phone);
				loginMember.setAddress(address);
			}else {
				System.out.println("수정 실패했습니다.");
			}

		}catch(Exception e) {
			System.out.println("정보 수정 과정에서 오류가 발생했습니다.");
			e.printStackTrace();
		}

	}



	/**작성자 강보령
	 * 회원 탈퇴용 View
	 */
	@Override
	public void updateSecessionMember() {
		System.out.println("**~회원 탈퇴~**");
		System.out.println("비밀번호를 입력해주세요 : ");
		String pw = sc.nextLine();

		String random = "";

		for(int i=0; i<6; i++) {
			char num = (char)(Math.random()*26+97);
			random += num;
		}

		System.out.print("정말 탈퇴를 진행하시겠습니까? \n 탈퇴를 원하시면 보안문자를 입력주세요 ["+ random + "] : " );
		String input = sc.nextLine();


		if(input.equals(random)) {
			try {
				Member member = new Member();

				member.setMemNo(loginMember.getMemNo());
				member.setMemPw(pw);

				int result = mService.updateSecessionMeber(member);

				if(result>0) {
					System.out.println("탈퇴되었습니다...ㅠㅠ");
					loginMember = null;
				}
				else	System.out.println("비밀번호가 일치하지 않습니다."); 

			}catch(Exception e) {
				System.out.println("회원 탈퇴 과정에서 오류가 발생했습니다.");
				e.printStackTrace();
			}

		}else {
			System.out.println("보안문자를 잘못 입력하셨습니다.");
		}



	}


}


```

```java
// MemberService.java
package manat.jurr.model.service;

import java.sql.Connection;
import java.util.List;

import static manat.jurr.common.JurrTemplate.*;

import manat.jurr.model.DAO.MemberDAO;
import manat.jurr.model.vo.Member;
import manat.jurr.model.vo.Product;
import manat.jurr.model.vo.VProduct;

public class MemberService {
	
	
	private MemberDAO mDAO = new MemberDAO();
	
	

	/** 회원가입용 Service
	 * 작성자 김만희
	 * @param newMember
	 * @return result
	 */ 
	public int signUp(Member newMember) throws Exception{
		
		Connection conn = getConnection();
		
		int result = mDAO.signUp(conn , newMember);
		
		
		if(result > 0)	commit(conn);
		else			rollback(conn);
		
		
		return result;
	}



	/** 로그인용 Service 
	 * 작성자 김만희
	 * @param member
	 * @return loginMember
	 * @throws Exception
	 */
	public Member loginMember(Member member)  throws Exception{
		
		Connection conn = getConnection();
		
		Member loginMember = mDAO.login(conn, member);
		
		close(conn);
		
		return loginMember;
	}


	
	
	/**작성자 강보령 
	 * 내가 쓴 글 전체 조회 serivce
	 * @param memId
	 * @return list
	 * @throws Exception
	 */
	public List<VProduct> selectMyBoard(String memId) throws Exception {
		Connection conn = getConnection();
		
		List<VProduct> list = mDAO.selectMyBoard(conn, memId);
		
		close(conn);
	
		return list;
	}



	/** 작성자 강보령 
	 * 내가 쓴 글 수정 service
	 * @param update
	 * @return result
	 * @throws Exception
	 */
	public int updateMyBoard(Product update) throws Exception {
		Connection conn = getConnection();
		
		int result = mDAO.updateMyBoard(conn, update);
		
		if(result >0) commit(conn);
		else 	rollback(conn);
		
		close(conn);
		
		return result;
	}



	/**작성자 강보령 
	 * 내 게시글 삭제 DAO
	 * @param itemNo
	 * @param memNo
	 * @return result
	 * @throws Exception
	 */
	public int deleteMyBoard(int itemNo) throws Exception {
		Connection conn = getConnection();
		
		int result =0;
		
		result = mDAO.deleteMyBoard(conn, itemNo);
		
		if(result>0) commit(conn);
		else rollback(conn);
				
		close(conn);
		
		return result;
	}



	/**작성자 강보령 
	 * 게시글 수정, 삭제 할때 본인글인지 확인용 Service
	 * @param product
	 * @return result
	 * @throws Exception
	 */
	public int checkMyBoard(Product product) throws Exception {
		Connection conn = getConnection();
		
		int result = mDAO.checkMyBoard(conn, product);
		close(conn);
	
		return result;
		
	}
	
	

	/**작성자 강보령 
	 * 나의 단군 레벨 조회 Service
	 * @param point
	 * @return lv
	 * @throws Exception
	 */
	public Member myDanKunLV(int memNo) throws Exception {
		Connection conn = getConnection();
		
		Member member = mDAO.myDanKunLV(conn, memNo);
		
		if(member != null) {
			
			
			//각설이0~20,평민20~40,선비40~60,장군60~80,왕80 ~99,단군 100
			if(member.getPoint() == 100) {
				member.setDankunLv("단군");
			}else if(member.getPoint()>=80) {
				member.setDankunLv("왕");
			}else if(member.getPoint()>=60) {
				member.setDankunLv("장군");
			}else if(member.getPoint()>=40) {
				member.setDankunLv("선비");
			}else if(member.getPoint()>=20) {
				member.setDankunLv("평민");
			}else {
				member.setDankunLv("각설이");
			}
			
			
			Member member1 = new Member();
			
			member1.setMemNo(memNo);
			member1.setPoint(member.getPoint());
			member1.setDankunLv(member.getDankunLv());
			int result = mDAO.updateDanKunLV(conn, member1);
			
			
			if(result >0) {
				
				member.setDankunLv(member1.getDankunLv());
				commit(conn);
			}else {
				rollback(conn);
			}
			
			close(conn);
		}

		return member;
	}
	
	



	/**작성자 강보령 
	 * 회원정보 수정 Service
	 * @param member
	 * @return result
	 * @throws Exception
	 */
	public int updateMyInfo(Member member) throws Exception {
		Connection conn = getConnection();
		
		int result = mDAO.updateMyInfo(conn, member);
		
		if(result>0) commit(conn);
		else rollback(conn);
		
		close(conn);
		return result;
	}



	/** 작성자 강보령
	 * 회원 탈퇴용 Service
	 * @param member
	 * @return result
	 * @throws Exception
	 */
	public int updateSecessionMeber(Member member) throws Exception {
		Connection conn = getConnection();
		
		int result = mDAO.updateSecessionMember(conn, member);
		
		if(result>0) 	commit(conn);
		else  rollback(conn);
		
		close(conn);
		
		return result;
	}


}


```


```java
//MemberDAO.java
package manat.jurr.model.DAO;

import static manat.jurr.common.JurrTemplate.*;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import manat.jurr.model.vo.Member;
import manat.jurr.model.vo.Product;
import manat.jurr.model.vo.VProduct;

public class MemberDAO {
	private Statement stmt = null;
	private PreparedStatement pstmt = null;
	private ResultSet rset = null;
	
	private Properties prop = null;
	
	public MemberDAO() {
		try {
			prop = new Properties();
			prop.loadFromXML(new FileInputStream("jurr-query.xml"));
		}catch (Exception e) {
			e.printStackTrace();
		}
	
	}
	
	/** 작성자 김만희
	 * 회원가입 DAO
	 * @param conn
	 * @param newMember
	 * @return result
	 * @throws Exception
	 */
	public int signUp(Connection conn, Member newMember) throws Exception{
		
		int result = 0; 
		
		try {
			String query = prop.getProperty("signUp");
			pstmt = conn.prepareStatement(query);
		
			pstmt.setString(1,newMember.getMemId());
			pstmt.setString(2,newMember.getMemPw());
			pstmt.setString(3,newMember.getMemNm());
			pstmt.setString(4,newMember.getPhone());
			pstmt.setString(5,newMember.getAddress());
			
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(conn);
		}
		return result;
	}


	/** 작성자 김만희
	 * 로그인 DAO
	 * @param conn
	 * @param member
	 * @return loginMember
	 * @throws Exception
	 */
	public Member login(Connection conn, Member member) throws Exception{
		
		Member loginMember = null;
		
		try {
			
		String query = prop.getProperty("login");
		
		pstmt = conn.prepareStatement(query);
		
		pstmt.setString(1, member.getMemId());
		pstmt.setString(2, member.getMemPw());
		
		rset = pstmt.executeQuery();
		
		if(rset.next()) {
			loginMember = new Member(rset.getInt("MEM_NO"), 
					 rset.getString("MEM_ID"),
					 rset.getString("MEM_NM"),
					 rset.getString("PHONE"), 
					 rset.getString("DANKUN_LV"),
					 rset.getInt("POINT"));
			
		}
		
		}finally {
			close(rset);
			close(pstmt);
		}
		
		return loginMember;
	}


	
	
	
	
	
	
	
	/** 작성자 강보령 
	 * 내가 쓴 글 전체 조회 DAO
	 * @param conn
	 * @param memId
	 * @return list
	 * @throws Exception
	 */
	public List<VProduct> selectMyBoard(Connection conn, String memId) throws Exception {
		List<VProduct> list = null;
						
		try {
			String query = prop.getProperty("selectMyBoard");
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, memId);
			
			rset = pstmt.executeQuery();
			list = new ArrayList<>();
			while(rset.next()) {
				list.add(new VProduct(rset.getInt("ITEM_NO"), rset.getString("MEM_ID"), rset.getString("ITEM_NM"), 
						rset.getInt("Price"), rset.getString("KIND")));
			}
			
		}finally {
			close(pstmt);
		}
		return list;
	}


	
	
	/** 작성자 강보령  
	 * 내가 쓴 글 수정 DAO
	 * @param conn
	 * @param update
	 * @return result
	 * @throws Exception
	 */
	public int updateMyBoard(Connection conn, Product update) throws Exception {
		int result = 0;
		
		try {
			String query = prop.getProperty("updateMyBoard");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, update.getItemNm());
			pstmt.setInt(2, update.getPrice());
			pstmt.setString(3, update.getContent());
			pstmt.setInt(4, update.getItemNo());
			
			result = pstmt.executeUpdate();
			
	
		}finally {
			close(pstmt);
		}
		return result;
	}




	/** 작성자 강보령  
	 * 내 게시글 삭제 DAO
	 * @param conn
	 * @param itemNo
	 * @param memNo
	 * @return result
	 * @throws Exception
	 */
	public int deleteMyBoard(Connection conn, int itemNo) throws Exception {
		int result = 0;
		
		try {
			String query = prop.getProperty("deleteMyBoard");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, itemNo);
			
			result = pstmt.executeUpdate();
		}finally {
			close(pstmt);
		}
		return result;
	}




	/**작성자 강보령 
	 * 게시글 수정, 삭제 할때 본인글인지 확인용 Service
	 * @param conn
	 * @param product
	 * @return result
	 * @throws Exception
	 */
	public int checkMyBoard(Connection conn, Product product)throws Exception {
		int result = 0;
		try {
			String query = prop.getProperty("checkMyBoard");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, product.getMemNo());
			pstmt.setInt(2, product.getItemNo());
			
			rset = pstmt.executeQuery();
			
			if(rset.next()) {
				result = rset.getInt(1);
			}
			
		}finally {
			close(rset);
			close(pstmt);
		}
		return result;
	}

	
	

	/** 작성자 강보령  
	 * 나의 단군 레벨 조회 DAO
	 * @param conn
	 * @param member
	 * @return result
	 * @throws Exception
	 */
	public Member myDanKunLV(Connection conn, int memNo) throws Exception {
		Member member = null;
		
		try {
			String query = prop.getProperty("myDanKunLV");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, memNo);
			
			rset = pstmt.executeQuery();
						
			if(rset.next()) {
				member = new Member(rset.getString(1), 
								rset.getInt(2), 
								rset.getString(3));
			}
			
		}finally {
			close(rset);
			close(pstmt);
		}
		return member;
	}
	


	/** 작성자 강보령  
	 * 단군 레벨 업데이트용 DAO
	 * @param conn
	 * @param member1
	 * @return result
	 * @throws Exception
	 */
	public int updateDanKunLV(Connection conn, Member member1) throws Exception {
		int result = 0;
		try {
			String query = prop.getProperty("updateDanKunLV");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, member1.getDankunLv());
			pstmt.setInt(2, member1.getMemNo());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}





	/** 작성자 강보령  
	 * 회원정보 수정 DAO
	 * @param conn
	 * @param member
	 * @return result
	 * @throws Exception
	 */
	public int updateMyInfo(Connection conn, Member member) throws Exception {
		int result =0;
		
		try {
			String query = prop.getProperty("updateMyInfo");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setString(1, member.getMemNm());
			pstmt.setString(2, member.getPhone());
			pstmt.setString(3, member.getAddress());
			pstmt.setInt(4, member.getMemNo());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}




	/**작성자 강보령  
	 * 회원 탈퇴용 DAO
	 * @param member
	 * @return result
	 * @throws Exception
	 */
	public int updateSecessionMember(Connection conn, Member member) throws Exception {
		int result =0;
		
		try { 
			String query = prop.getProperty("updateSecessionMember");
			
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, member.getMemNo());
			pstmt.setString(2, member.getMemPw());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		return result;
	}


}


```
```java
//ProductService.java
package manat.jurr.model.service;

import static manat.jurr.common.JurrTemplate.close;
import static manat.jurr.common.JurrTemplate.commit;
import static manat.jurr.common.JurrTemplate.getConnection;
import static manat.jurr.common.JurrTemplate.rollback;

import java.sql.Connection;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

import manat.jurr.model.DAO.ProductDAO;
import manat.jurr.model.vo.Product;
import manat.jurr.model.vo.VProduct;

public class ProductService {
	
	private ProductDAO pDAO = new ProductDAO(); 
	private Scanner sc = new Scanner(System.in);
	
	
	
	/** 작성자 김만희
	 * 상품등록 Service
	 * @param product
	 * @return result
	 * @throws Exception
	 */
	public int registerProduct(Product product) throws Exception{
		
		Connection conn = getConnection();
		
		int result = pDAO.registerProduct(conn, product);
		
		if(result > 0 ) commit(conn);
		else			rollback(conn);
		
		close(conn);
		
		return result;
	}

	/** 모든 판매글 간단 조회 service
	 * @author 박혜윤
	 * @return List
	 * @throws Exception
	 */
	public List<VProduct> selectAllItem() throws Exception {
		Connection conn = getConnection();
		
		List<VProduct> list = pDAO.selectAllItem(conn);
		close(conn);
		return list;
	}

	/** 검색으로 판매글 상세 조회 service
	 * @author 박혜윤
	 * @param map
	 * @return List
	 * @throws Exception
	 */
	public List<VProduct> searchKeyword(Map<String, Object> map) throws Exception {
	      Connection conn = getConnection();
	      
	      
	      String query = "SELECT * FROM V_PRODUCT WHERE";
	      String like = "Like '%" + map.get("keyword") + "%'";
	      String priceRange = "BETWEEN " + map.get("keyword");
	      
	      
	      switch((int)map.get("sel")) {
	      case 1 : query += " ITEM_NM " + like; break; //상품명 조회
	      case 2 : query += " KIND " + like; break; //카테고리 조회
	      case 3 : 
	         System.out.print(map.get("keyword") + " 원 이상 ");
	         String keyword2 = sc.nextLine();
	         map.put("keyword2", keyword2);
	         query += " PRICE " + priceRange  + " AND "  + map.get("keyword2"); break; //가격으로 조회
	      case 4 : query += " ADDRESS " + like; break; //위치로 조회
	      }
	      query += " AND DELETE_FL='N' ORDER BY ITEM_NO DESC";
	      
	      List<VProduct> list = pDAO.searchKeyword(conn, query);
	      close(conn);
	      
	      return list;
	   }

	/** 상품번호로 판매글 간단 조회 service
	 * @author 박혜윤
	 * @param num
	 * @return VProduct
	 * @throws Exception
	 */
	public VProduct searchItem(int num) throws Exception {
		Connection conn = getConnection();
		VProduct product = pDAO.searchItem(conn, num);
		return product;
	}

	/** 상품번호로 상품 구매 service
	 * @param num
	 * @return (int)result
	 * @throws Exception
	 */
	public int purchaseItem(int num) throws Exception {
		Connection conn = getConnection();
		int result = pDAO.purchaseItem(conn, num);
		
		if(result>0) commit(conn);
		else		 rollback(conn);
		
		return result;
	}



}


```
```java
//ProductDAO.java
package manat.jurr.model.DAO;

import static manat.jurr.common.JurrTemplate.*;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import manat.jurr.model.vo.Product;
import manat.jurr.model.vo.VProduct;
import manat.jurr.view.JurrView;

public class ProductDAO {
	private Statement stmt = null;
	private PreparedStatement pstmt = null;
	private ResultSet rset = null;
	
	private Properties prop = null;
	
	public ProductDAO() {
		try {
			prop = new Properties();
			prop.loadFromXML(new FileInputStream("jurr-query.xml"));
		}catch (Exception e) {
			e.printStackTrace();
		}
	
	}
	
	
	/** 
	 * 상품등록 DAO
	 * 작성자 김만희
	 * @param conn
	 * @param product
	 * @return result
	 * @throws Exception
	 */
	public int registerProduct(Connection conn, Product product) throws Exception{
		int result = 0;
		
		try {
			String query = prop.getProperty("registerProduct");
			
			pstmt = conn.prepareStatement(query);
			
			pstmt.setString(1, product.getItemNm());
			pstmt.setInt(2, product.getPrice());
			pstmt.setString(3, product.getKind());
			pstmt.setString(4, product.getAddress());
			pstmt.setString(5, product.getContent());
			pstmt.setInt(6, JurrView.loginMember.getMemNo());
			
			result = pstmt.executeUpdate();
			
		}finally {
			close(pstmt);
		}
		
		return result;
	}

	
	
	/** 모든 판매글 간단 조회 DAO
	 * @author 박혜윤
	 * @param conn
	 * @return List
	 * @throws Exception
	 */
	public List<VProduct> selectAllItem(Connection conn) throws Exception {
		List<VProduct> list = null;
		
		try {
			String query = prop.getProperty("selectAllItem");
			stmt = conn.createStatement();
			rset = stmt.executeQuery(query);
			list = new ArrayList<VProduct>();
			
			while(rset.next()) {
				list.add(new VProduct(
						rset.getString("MEM_ID"),
						rset.getString("ITEM_NM"),
						rset.getInt("PRICE"),
						rset.getString("KIND"),
						rset.getInt("ITEM_NO")));
			}
		} finally {
			close(rset);
			close(stmt);
		}
		
		return list;
		
	}

	/** 검색으로 판매글 상세 조회 DAO
	 * @author 박혜윤
	 * @param conn
	 * @param query
	 * @return List
	 * @throws Exception
	 */
	public List<VProduct> searchKeyword(Connection conn, String query) throws Exception {
		List<VProduct> list = null;
		
		try {
		stmt = conn.createStatement();
		rset = stmt.executeQuery(query);
		
		list = new ArrayList();
		while(rset.next()) {
			list.add(new VProduct(rset.getString("MEM_ID"),
								rset.getString("ITEM_NM"),
								 rset.getInt("PRICE"),
								 rset.getString("KIND"),
								 rset.getString("ADDRESS"),
								 rset.getInt("ITEM_NO"),
								 rset.getString("CONTENT")));
		}
		
		} finally {
			close(rset);
			close(stmt);
		}
		
		return list;
	}

	/** 상품번호로 판매글 간단 조회 DAO
	 * @author 박혜윤
	 * @param conn
	 * @param num
	 * @return VProduct
	 * @throws Exception
	 */
	public VProduct searchItem(Connection conn, int num) throws Exception {
		VProduct product = null;
		try {
			String query = prop.getProperty("selectSearchItem");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, num);
			rset = pstmt.executeQuery();
			
			if(rset.next()) {
				product = new VProduct(
						rset.getString("MEM_ID"),
						rset.getString("ITEM_NM"),
						rset.getInt("PRICE"),
						rset.getString("KIND"),
						rset.getInt("ITEM_NO"));
			}
		} finally {
			close(rset);
			close(pstmt);
		}
		return product;
	}
	
	

	/** 상품번호로 상품 구매 DAO
	 * @author 박혜윤
	 * @param conn
	 * @param num
	 * @return (int)result
	 * @throws Exception
	 */
	public int purchaseItem(Connection conn, int num) throws Exception {
		int result = 0;
		
		try {
		//판매글 삭제를 N -> Y로 수정
		String query = prop.getProperty("purchaseItem");
		pstmt = conn.prepareStatement(query);
		pstmt.setInt(1, num);

		result = pstmt.executeUpdate();
		
		if(result>0) {
			System.out.println(earnPoints(conn, num));
		}
		
		}finally {
			close(pstmt);
		}
		return result;
	}
	
	/** 구매 성공시 포인트 적립 DAO
	 * @param conn
	 * @param num
	 * @return (String)message
	 */
	public String earnPoints(Connection conn, int num) {
		String message = "";
		int result = 0;
		try {
			String query = prop.getProperty("earnPoints");
			pstmt = conn.prepareStatement(query);
			pstmt.setInt(1, num);
			result = pstmt.executeUpdate();
			
			if(result>0) {
				message = "구매 감사합니다. 구매하신 상품의 마켓 점수가 5점 상승되었습니다.";
			} else {
				message = "포인트 적립이 정상적으로 처리되지 않았습니다. 고객센터 1555-5555";
			}
			
		} catch(Exception e) {
			message = "포인트 적립 중 문제가 발생하였습니다. 고객센터 1555-5555";
			e.printStackTrace();
		}
		return message;
	}

}


```

<br/><br/>