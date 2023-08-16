---
layout: post
title: 파일업로드
categories: 그냥저냥
---

## 파일업로드

우선 파일 업로드를 위해서 cos API를 다운받아<br>

시작은 html로 할건데, 이름은 fileMain.html<br>
```1=fileMain.html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>파일 업로드</title>
</head>
<body>
	<a href="UploadServlet?type=1">이미지 파일 업로드(웹 경로 내부)</a><br>
	<a href="UploadServlet?type=2">문서 파일 업로드(서버 D드라이버에)</a><br>
	<a href="UploadServlet?type=3">이미지 파일 업로드(서버 D드라이버에)</a><br>
</body>
</html>
```

이미지 파일 업로드 링크를 타고가면 서블릿에 요청(UploadServlet?type=1)이 가겠지?<br>

```2=UploadServlet
@WebServlet("/UploadServlet")
public class UploadServlet extends HttpServlet {
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//encoding 생략
		String type=request.getParameter("type");
		if(type.equals("1")) {
			//webapp/file1/upload_01.jsp 이동
			RequestDispatcher rd=request.getRequestDispatcher("/file1/upload_01.jsp");
			rd.forward(request, response);
		}
	}
}
```
doGet 메서드를 오버라이딩해줄건데, queryString으로 type=1 , 2 , 3 이런식으로 날렸으니까<br>
request.getParameter("type"); 이렇게 받아주면 되겠지?<br>
그 타입이 1이라면, webapp아래의 file1폴더에 있는 upload_01.jsp로 포워딩할거야<br>

이제 upload_01.jsp로 넘어왔어<br>
```3=upload_01.jsp
<form action="upload1" method="post" enctype="multipart/form-data"  id="uploadForm">
//폼의 데이터를 넘기는 데 우리는 텍스트만 보내는게 아니라 파일도 보낼거기때문에<br>
//enctype="multipart/form-data" 얘를 꼭 써줘야해<br>
//그리고 2mb이상의 파일은 올리지 못하도록 js로 설정해줬어(js/fileupload_validate.js) <br>
```

이제 데이터를 넘기면 (@upload1)어노테이션이 걸린 서블릿으로 요청이 가겠찌?<br>
```4=FileUploadServlet1.java
@WebServlet("/upload1")
public class FileUploadServlet1 extends HttpServlet {
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//encoding 2가지
		request.setCharacterEncoding("utf-8");
		//넘어온 파라메타 값 받기 (***우리는 enctype="multipart/form-data" 얘를 걸었기 때문에 일반 request로는 받을 수 없고,
    //MultipartRequest 이친구로 받아야해
		//String name=request.getParameter("name").trim();
		//String title=request.getParameter("title").trim();
		//cos 라이브러리 사용 파일 업로드 처리
		//upload 할 폴더 설정
		String upload="upload/img";
		ServletContext context=this.getServletContext();
		String uploadPath=context.getRealPath(upload); //우리가 보는 폴더가 아니라, 실제 이클립스가 작동할 때 찾는 폴더 경로야
		System.out.println("uploadPath="+uploadPath);
		File path=new File(uploadPath);
		if(!path.exists()) {
			path.mkdirs(); //폴더가 없다면 디렉토리를 만들어라!
		}
		MultipartRequest multi=new MultipartRequest(request, uploadPath,2*1024*1024,
													"utf-8",new DefaultFileRenamePolicy());
    //생성자들이 각각 무슨역할을 할까?
    //1.request : 넘어온 파라메타를 받을 수 있게 해줘. multi.getParameter("??");
    //2.uploadPath : 업로드할 경로 지정
    //3.업로드될 파일의 최대 크기
    //4.인코딩
    //5.업로드한 파일의 이름이 같을 때 알아서 바꿔서 저장해주는 역할을 하는 객체야

		if(multi!=null) {
			String ofilename=multi.getOriginalFileName("file");
			System.out.println("getOriginalFileName="+ofilename);
			String systemfilename=multi.getFilesystemName("file");
			System.out.println("getFilesystemName="+systemfilename);
			String name=multi.getParameter("name");
			String title=multi.getParameter("title");
			System.out.println(name+"<>"+title);
			request.setAttribute("name", name);
			request.setAttribute("title", title);
			request.setAttribute("originFname", ofilename);
			request.setAttribute("saveFname", systemfilename);
			request.setAttribute("upload", upload);
			RequestDispatcher rd=request.getRequestDispatcher("file1/image-view.jsp");
			rd.forward(request, response);
		}
	}
}

```

