---
title: "[동물병원] 사용된 VO"
excerpt: "세미프로젝트 - 뭉개뭉개"
search: true
categories: 
  - SemiProject
tags: 
  - JSP
  - Servlet
  - Java
  - HTML
  - CSS
toc: true
---

## 뭉개뭉개 동물병원 VO


- 동물병원

```java
package com.kh.semi.hospital.model.vo;


public class Hospital {
	private int hospNo;
	private String hospNm;
	private String location1;
	private String location2;
	private String phone;
	private String openingTime;
	private String closingTime;
	private String hospInfo;
	private int viewCount;
	private String hospDelFl;
	private int memNo;
	private String  hospFacility;
	
	public Hospital() {}
	
	
//	list 조회 용 생성자
	public Hospital(int hospNo, String hospNm, String location2, String phone, String openingTime, String closingTime) {
		super();
		this.hospNo = hospNo;
		this.hospNm = hospNm;
		this.location2 = location2;
		this.phone = phone;
		this.openingTime = openingTime;
		this.closingTime = closingTime;
	}

	
	// 상세조회용 생성자
	public Hospital(String hospNm, String location2, String phone, String openingTime, String closingTime,
			String hospInfo, int viewCount, String hospFacility) {
		super();
		this.hospNm = hospNm;
		this.location2 = location2;
		this.phone = phone;
		this.openingTime = openingTime;
		this.closingTime = closingTime;
		this.hospInfo = hospInfo;
		this.viewCount = viewCount;
		this.hospFacility = hospFacility;
	}


	
	// 모든 매개변수
	public Hospital(int hospNo, String hospNm, String location1, String location2, String phone, String openingTime,
				String closingTime, String hospInfo, int viewCount, String hospDelFl, int memNo, String hospFacility) {
			super();
			this.hospNo = hospNo;
			this.hospNm = hospNm;
			this.location1 = location1;
			this.location2 = location2;
			this.phone = phone;
			this.openingTime = openingTime;
			this.closingTime = closingTime;
			this.hospInfo = hospInfo;
			this.viewCount = viewCount;
			this.hospDelFl = hospDelFl;
			this.memNo = memNo;
			this.hospFacility = hospFacility;
		}
	
	
	
	
	
	public int getHospNo() {
		return hospNo;
	}
	
	
	public void setHospNo(int hospNo) {
		this.hospNo = hospNo;
	}
	
	
	public String getHospNm() {
		return hospNm;
	}
	
	
	public void setHospNm(String hospNm) {
		this.hospNm = hospNm;
	}
	
	
	public String getLocation1() {
		return location1;
	}
	
	
	public void setLocation1(String location1) {
		this.location1 = location1;
	}
	
	
	public String getLocation2() {
		return location2;
	}
	
	
	public void setLocation2(String location2) {
		this.location2 = location2;
	}
	
	
	public String getPhone() {
		return phone;
	}
	
	
	public void setPhone(String phone) {
		this.phone = phone;
	}
	
	
	public String getOpeningTime() {
		return openingTime;
	}
	
	
	public void setOpeningTime(String openingTime) {
		this.openingTime = openingTime;
	}
	
	
	public String getClosingTime() {
		return closingTime;
	}
	
	
	public void setClosingTime(String closingTime) {
		this.closingTime = closingTime;
	}
	
	
	public String getHospInfo() {
		return hospInfo;
	}
	
	
	public void setHospInfo(String hospInfo) {
		this.hospInfo = hospInfo;
	}
	
	
	public int getViewCount() {
		return viewCount;
	}
	
	
	public void setViewCount(int viewCount) {
		this.viewCount = viewCount;
	}
	
	
	public String getHospDelFl() {
		return hospDelFl;
	}
	
	
	public void setHospDelFl(String hospDelFl) {
		this.hospDelFl = hospDelFl;
	}
	
	
	public int getMemNo() {
		return memNo;
	}
	
	
	public void setMemNo(int memNo) {
		this.memNo = memNo;
	}
	
	
	public String getHospFacility() {
		return hospFacility;
	}
	
	
	public void setHospFacility(String hospFacility) {
		this.hospFacility = hospFacility;
	}
	
	
	@Override
	public String toString() {
		return "Hospital [hospNo=" + hospNo + ", hospNm=" + hospNm + ", location1=" + location1 + ", location2=" + location2
				+ ", phone=" + phone + ", openingTime=" + openingTime + ", closingTime=" + closingTime + ", hospInfo="
				+ hospInfo + ", viewCount=" + viewCount + ", hospDelFl=" + hospDelFl + ", memNo=" + memNo
				+ ", hospFacility=" + hospFacility + "]";
	}

}
```
<br><br>

- 페이징

```java
package com.kh.semi.hospital.model.vo;

public class PageInfo { // 페이징 처리를 위한 값을 저장할  객체
	
	// 얻어 올 값
	private int currentPage; // 현재 페이지 번호를 저장할 변수
	private int listCount; // 전체 게시글 수를 저장할 변수
	
	// 설정할 값
	private int limit = 6; // 한 페이지에 보여질 게시글 목록 수 
	private int pageSize = 10; // 페이징바에 표시될 페이지 수
	
	
	// 계산할 값
	private int maxPage;    // 전체 목록 페이지의 수 == 마지막 페이지
	private int startPage;  // 페이징바 시작 페이지 번호
	private int endPage;    // 페이징바 끝 페이지 번호
	
	// 기본 생성자 사용 X
	
	
	public PageInfo(int currentPage, int listCount) {
		super();
		this.currentPage = currentPage;
		this.listCount = listCount;
		
		// 전달받은 값과 명시적으로 선언된 값을 이용하여 
		// makePageInfo()수행
		// vo가 생성되면 계산이 완료 된다.
		makePageInfo();
	}

	public PageInfo(int currentPage, int listCount, int limit, int pageSize) {
		super();
		this.currentPage = currentPage;
		this.listCount = listCount;
		this.limit = limit;
		this.pageSize = pageSize;
		
		makePageInfo();
	}
	
	

	public int getCurrentPage() {
		return currentPage;
	}

	public void setCurrentPage(int currentPage) {
		this.currentPage = currentPage;
	}

	public int getListCount() {
		return listCount;
	}

	public void setListCount(int listCount) {
		this.listCount = listCount;
		makePageInfo();
	}

	public int getLimit() {
		return limit;
	}

	public void setLimit(int limit) {
		this.limit = limit;
		makePageInfo();
	}

	public int getPageSize() {
		return pageSize;
	}

	public void setPageSize(int pageSize) {
		this.pageSize = pageSize;
		makePageInfo();
	}

	public int getMaxPage() {
		return maxPage;
	}

	public void setMaxPage(int maxPage) {
		this.maxPage = maxPage;
	}

	public int getStartPage() {
		return startPage;
	}

	public void setStartPage(int startPage) {
		this.startPage = startPage;
	}

	public int getEndPage() {
		return endPage;
	}

	public void setEndPage(int endPage) {
		this.endPage = endPage;
	}

	@Override
	public String toString() {
		return "PageInfo [currentPage=" + currentPage + ", listCount=" + listCount + ", limit=" + limit + ", pageSize="
				+ pageSize + ", maxPage=" + maxPage + ", startPage=" + startPage + ", endPage=" + endPage + "]";
	}
	
	
	// 페이징 처리에 필요한 값을 계산하는 메소드
	// private: 내부적으로만 사용 가능
	private void makePageInfo() {
		// maxPage : 총 페이지 수 == 마지막 페이지
		// 총 게시글 수가 60개, 한 페이지에 보여지는 게시글 수 6개 ==>  총 페이지 수 = 10, 마지막 페이지 = 10
		// 총 게 시글 수 101개, 한 페이지에 보여지는 게시글 수 6개 ==> 총 페이지 수 == (16.8 올림처리)17, 마지막 페이지 = 17
		maxPage = (int)Math.ceil((double)listCount / limit);
		
		
		// startPage: 페이징바 시작 번호
		// 페이징바에 페이지를 10개씩 보여줄 경우   1,11,21,31...
		startPage = (currentPage-1) / pageSize * pageSize + 1;
					// (11-1) / 10 * 10 + 1;
		
		// endPage : 페이징바의 끝 번호
		// 페이징바에 페이지를 10개씩 보여줄 경우 10,20,30,40 ...
		endPage = startPage + pageSize - 1;
		
		// 총 페이지의 수가 end페이지보다 작을 경우
		if(maxPage<=endPage) {
			endPage = maxPage;
		}
	}

}
```
<br><br>

- 첨부파일

```java
package com.kh.semi.hospital.model.vo;

public class Attachment {
	private int fileNo;
	private String filePath;
	private String fileName;
	private int fileLevel;
	private int hospNo;

	public Attachment() {}
	
	

	public Attachment(int fileNo, String fileName, int fileLevel) {
		super();
		this.fileNo = fileNo;
		this.fileName = fileName;
		this.fileLevel = fileLevel;
	}



	public Attachment(int fileNo, String filePath, String fileName, int fileLevel, int hospNo) {
		super();
		this.fileNo = fileNo;
		this.filePath = filePath;
		this.fileName = fileName;
		this.fileLevel = fileLevel;
		this.hospNo = hospNo;
	}



	public int getFileNo() {
		return fileNo;
	}



	public void setFileNo(int fileNo) {
		this.fileNo = fileNo;
	}



	public String getFilePath() {
		return filePath;
	}



	public void setFilePath(String filePath) {
		this.filePath = filePath;
	}



	public String getFileName() {
		return fileName;
	}



	public void setFileName(String fileName) {
		this.fileName = fileName;
	}



	public int getFileLevel() {
		return fileLevel;
	}



	public void setFileLevel(int fileLevel) {
		this.fileLevel = fileLevel;
	}



	public int getHospNo() {
		return hospNo;
	}



	public void setHospNo(int hospNo) {
		this.hospNo = hospNo;
	}

	@Override
	public String toString() {
		return "Attachment [fileNo=" + fileNo + ", filePath=" + filePath + ", fileName=" + fileName + ", fileLevel="
				+ fileLevel + ", hospNo=" + hospNo + "]";
	}

}
```

<br><br>
