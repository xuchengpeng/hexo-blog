---
title:  "SourceInsight代码检视"
date: 2017-11-20 17:10:00
updated: 2017-11-20 17:10:00
comments: true
categories: 
  - Technology
tags:
  - SourceInsight
  - CodeReview
---

这是一个SourceInsight使用的本地代码检视工具，利用Macro实现。

<!-- more -->

## 创建CodeReview.em文件
将代码保存为CodeReview.em文件。

## 添加到base工程
将CodeReview.em文件复制到base工程目录下。

## 设置快捷键
在SourceInsight中设置“Review_Add_Comment”、“Review_Restore_Link”和“Review_Summary”等相关宏的快捷键即可使用。

```em
/**
 * 
 * Code Review Tool
 * 
 * Author: FN QD
 * 
 * Amendment List:
 *
 *   2003.1.22 Javey 
 * 1) Changed severity level according to the review form
 * 2) Excluded the category of "suggestion" when counting 
 *  total defects by category to comply with the review form
 *   2003.5.14 Javey 
 *1) Forced reviewers to fill category and defect type.
 *2) Removed these categories:SysReq,SDes,Docs and Others
 *  from summary section
 */

macro GetReviewToolVersion()
{
    return "2.0.2"
}

macro GetReviewDate()
{
    SysTime = GetSysTime(1)
    szYear=SysTime.Year
    szMonth=SysTime.month
    szDay=SysTime.day

    return "@szYear@-@szMonth@-@szDay@"
}

macro Review_Restore_Link()
{
    hbuf = GetCurrentBuf()

    //sProjRoot = GetProjDir(GetCurrentProj())
    sProjRoot = Cat(sProjRoot, "\\")

    line = 0
    while(True)
    {
        sel = SearchInBuf(hbuf, "FileName : ", line, 0, 1, 0, 0)
        if(sel == "") break

        line = sel.lnFirst
        col = sel.ichLim
        str = GetBufLine(hbuf, line)
        fileName = strmid(str, col, strlen(str))
        //fileName = cat(sProjRoot, fileName)

        str = GetBufLine(hbuf, line+1)
        lnNumber = strmid(str, 11, strlen(str))
        SetSourceLink(hbuf, line, fileName, lnNumber - 1)
        line = line+2
    }

    //updateSummary(hbuf)
}


macro Review_Add_Comment()
{
    hbuf = GetCurrentBuf()
    curFileName = GetBufName(hbuf)
    sProjRoot = GetProjDir(GetCurrentProj())
    //nPos = strlen(sProjRoot)
    //sFileName = strmid(curFileName, nPos+1, strlen(curFileName))

 
    //ln从0开始编号
    curLineNumber = GetBufLnCur(hbuf) 
    totalline = GetBufLineCount (hbuf)    
    curFunc = nil
    symbolrecord = nil
    cTempLine = curLineNumber    
    
    /*下面处理主要解决空行、注释行symbol为空的问题*/
    /*GetSymbolLocationFromLn参数ln是从0编号*/
    while ((symbolrecord == nil) && (cTempLine<totalline))
    {

         symbolrecord = GetSymbolLocationFromLn(hbuf,cTempLine)
         if (symbolrecord != nil)
         {
             curFunc = symbolrecord.Symbol
             /*对非func类的symbol，一律都显示空*/
             if ((curFunc != nil) && (symbolrecord.Type != Function)
             {    
                 curFunc = nil
             }
             break;
         }                 
         cTempLine=cTempLine+1         
    }


    sFileName   = cat( "FileName : ", curFileName )
    sLineNumber = cat( "Line     : ", curLineNumber + 1 )

    /* get the severity of the current comment */
    promote = "Severity : M,m(Major严重); G,g(General一般); S,s(Suggestion提示)"
    sTemp = ask(promote);
    sTemp = toupper(sTemp[0]);
    while( sTemp != "M" && sTemp != "G" && sTemp != "S" )
    {
        sTemp = ask(cat("Please input again! ", promote));
        sTemp = toupper(sTemp[0]);
    }

    if( sTemp == "M" ) sTemp = "Major";
        else if ( sTemp == "G" ) sTemp = "General";
        else if ( sTemp == "S" ) sTemp = "Suggestion";

    sSeverity = cat( "Severity : ", sTemp );


    promote = "Category : 1.编程规范(代码级); 2.非编码规范(代码级); 3.业务功能; 4.软件结构"
    sTemp = ask(promote);

    
    ChoseNumber = AsciiFromChar(sTemp);
    MinNumber = AsciiFromChar("1");
    MaxNumber = AsciiFromChar("4");

    while ((ChoseNumber < MinNumber) || (ChoseNumber > MaxNumber))
    {
        sTemp = ask(promote);
        sTemp = toupper(sTemp[0]);
        ChoseNumber = AsciiFromChar(sTemp);
    }
  
    if( sTemp == "1" ) 
    {
        sCategory = cat( "Category : ", "编程规范");
    }
    else if ( sTemp == "2" )
    {
        sCategory = cat( "Category : ", "非编码规范");
    }
    else if ( sTemp == "3" )
    {
        sCategory = cat( "Category : ", "业务功能");
    }
    else if ( sTemp == "4" )
    {
        sCategory = cat( "Category : ", "软件结构");
    }

    
    if (sTemp == "1") // 编程规范
    {
     
        promote = "1.排版/命名 2.注释/可读性 3.常量左值/魔鬼数字 4.断言调试/打印"
  
        sTemp = ask(promote);
        sTemp = toupper(sTemp[0]);
        ChoseNumber = AsciiFromChar(sTemp);
        MinNumber = AsciiFromChar("1");
        MaxNumber = AsciiFromChar("4");

        while ((ChoseNumber < MinNumber) || (ChoseNumber > MaxNumber))
        {
            sTemp = ask(promote);
            sTemp = toupper(sTemp[0]);
            ChoseNumber = AsciiFromChar(sTemp);
        }
        EndMsg();

        if( sTemp == "1" ) sTemp = "排版/标识符命名";
            else if ( sTemp == "2" ) sTemp = "注释/可读性";
            else if ( sTemp == "3" ) sTemp = "常量左值/魔鬼数字";
            else if ( sTemp == "4" ) sTemp = "断言调试/打印";

        sType = cat( "Type     : ", sTemp );
    }
    else if (sTemp == "2") // 非编码规范
    {
     
        promote = "1.参数/返回值/优先级 2.变量初始化和赋值 3.资源申请释放 4.中断/效率/移植性 5.循环/if/switch 6.结构/宏/枚举 7.函数逻辑 8.其他";
        
        sTemp = ask(promote);
        sTemp = toupper(sTemp[0]);
        ChoseNumber = AsciiFromChar(sTemp);
        MinNumber = AsciiFromChar("1");
        MaxNumber = AsciiFromChar("8");

        

        while ((ChoseNumber < MinNumber) || (ChoseNumber > MaxNumber))
        {
            sTemp = ask(promote);
            sTemp = toupper(sTemp[0]);
            ChoseNumber = AsciiFromChar(sTemp);
        }
        EndMsg();

        if( sTemp == "1" ) sTemp = "函数参数/返回值检查/操作符优先级";
            else if ( sTemp == "2" ) sTemp = "变量/指针/数组/字符串初始化和赋值";
            else if ( sTemp == "3" ) sTemp = "资源申请和释放";
            else if ( sTemp == "4" ) sTemp = "中断/效率/可移植性";
            else if ( sTemp == "5" ) sTemp = "循环/if/switch程序块";
            else if ( sTemp == "6" ) sTemp = "结构/宏/枚举定义和使用";
            else if ( sTemp == "7" ) sTemp = "函数逻辑控制";
            else if ( sTemp == "8" ) sTemp = "其他";

            sType = cat( "Type     : ", sTemp );
    }
    else if ( sTemp == "3" ) // 业务功能
    {
        promote = "1.功能需求遗漏 2.功能需求没有正确实现"
        
        sTemp = ask(promote);
        sTemp = toupper(sTemp[0]);
        ChoseNumber = AsciiFromChar(sTemp);
        MinNumber = AsciiFromChar("1");
        MaxNumber = AsciiFromChar("2");

        while ((ChoseNumber < MinNumber) || (ChoseNumber > MaxNumber))
        {
            sTemp = ask(promote);
            sTemp = toupper(sTemp[0]);
            ChoseNumber = AsciiFromChar(sTemp);
        }
        EndMsg();

        if( sTemp == "1" ) sTemp = "功能需求遗漏";
        else if ( sTemp == "2" ) sTemp = "功能需求没有正确实现";


        sType = cat( "Type     : ", sTemp );    
    }
    else if ( sTemp == "4" ) // 软件结构
    {
        promote = "1.函数功能不单一 2.函数复杂度高 3.数据依赖多(全局，局部变量，参数) 4.全局变量访问不合理 5.冗余重复代码 6.其他"
        
        sTemp = ask(promote);
        sTemp = toupper(sTemp[0]);
        ChoseNumber = AsciiFromChar(sTemp);
        MinNumber = AsciiFromChar("1");
        MaxNumber = AsciiFromChar("6");

        while ((ChoseNumber < MinNumber) || (ChoseNumber > MaxNumber))
        {
            sTemp = ask(promote);
            sTemp = toupper(sTemp[0]);
            ChoseNumber = AsciiFromChar(sTemp);
        }
        EndMsg();

        if( sTemp == "1" ) sTemp = "函数功能不单一";
        else if ( sTemp == "2" ) sTemp = "函数复杂度高";
        else if ( sTemp == "3" ) sTemp = "数据依赖多(全局，局部变量，参数)";
        else if ( sTemp == "4" ) sTemp = "全局变量访问不合理";
        else if ( sTemp == "5" ) sTemp = "冗余重复代码"; 
        else if ( sTemp == "6" ) sTemp = "其他";         
        sType = cat( "Type     : ", sTemp );    
    }    
    

    /* get the comment */
    promote = "Input your comment:"
    sTemp = ask(promote);
    sComments = cat( "Comments : ", sTemp );

    /* get the licence user name for the reviewer name */
    progRecord = GetProgramEnvironmentInfo()
    sMyName = progRecord.UserName
    szReviewDate = GetReviewDate()
    szReviewFileName = "ReviewComment_@sMyName@_@szReviewDate@.txt"
    
    /* get the ReviewComment buffer handle */
    bNewCreated = false; // used for the review comment is firstly created

    hout = GetBufHandle(szReviewFileName)
    if (hout == hNil)
    {
        // No existing Review Comment buffer
        hout= OpenBuf (szReviewFileName)
        if( hout == hNil )
        {
            /* No existing ReviewComment.txt, then create a new review comment buffer */
            hout = NewBuf(szReviewFileName)
            NewWnd(hout)
            bNewCreated = true

            /*----------------------------------------------------------------*/
            /* Get the owner's name from the environment variable: MYNAME.    */
            /* If the variable doesn't exist, then the owner field is skipped.*/
            /*----------------------------------------------------------------*/

            AppendBufLine(hout, "[Review Info] ")
            AppendBufLine(hout, cat("ReviewTool Version    : ", GetReviewToolVersion()))
            AppendBufLine(hout, cat("Reviewer Name         : ", sMyName))
            sDate = GetSysTime(1)
            AppendBufLine(hout, cat("Review   Date         : ", sDate.date))
		    /* Size summary */
		    AppendBufLine(hout, "Review size(KLOC)     : ")              
		    /* Effort summary */
		    AppendBufLine(hout, "Review effort(P*Hours): ")
		    AppendBufLine(hout, "Rework effort(P*Hours): ")    	    
            AppendBufLine(hout, "-------------------------------------------------------------------------")
            AppendBufLine(hout, "")

        }
    } // end of get ReviewComment buffer handle

    delSummary(hout)
    AppendBufLine(hout, sFileName)
    AppendBufLine(hout, sLineNumber)
    AppendBufLine(hout, cat("Reviewer : ", sMyName))
    AppendBufLine(hout, cat("Symbol   : ", curFunc) )
    AppendBufLine(hout, sSeverity)
    AppendBufLine(hout, "Status   : Open")
    AppendBufLine(hout, sComments)
    AppendBufLine(hout, "Resolve  : ")
    AppendBufLine(hout, sCategory)
    AppendBufLine(hout, sType)
    AppendBufLine(hout, "Author   : ")
    AppendBufLine(hout, "Phase    : ")
    AppendBufLine(hout, "")

    lnSource = GetBufLineCount(hout) - 12
    SetSourceLink(hout, lnSource, curFileName, curLineNumber)
    updateSummary(hout)
    if( bNewCreated ) SetCurrentBuf(hbuf)
    jump_to_link;
}

macro Review_Summary()
{
    hbuf = GetCurrentBuf()
 
    updateSummary(hbuf)
}

macro updateSummary(hbuf)
{
    rvSum0 = getReviewSummary(hbuf)
    rvSum = "major=\"0\";general=\"0\";suggestion=\"0\";open=\"0\";closed=\"0\";rejected=\"0\";duplicate=\"0\";discuss=\"0\";SysReq=\"0\";SDes=\"0\";SRS=\"0\";HLD=\"0\";LLD=\"0\";TP=\"0\";Code=\"0\";Docs=\"0\";Others=\"0\""

    /* summary the severity */
    ln = 0
    while (True)
    {
        sel = SearchInBuf(hbuf, "^Severity\\s+:\\s+", ln, 0, 1, 1, 0)
        if (sel == null) break

        ln = sel.lnFirst
        col = sel.ichLim
        s = GetBufLine(hbuf, ln)
        sTemp = strmid(s, col, col+1)
        sTemp = toupper(sTemp);

        if (sTemp == "M" && validDefect(hbuf, ln)) 
            rvSum.major = rvSum.major + 1
        else if (sTemp == "G" && validDefect(hbuf, ln))
            rvSum.general = rvSum.general + 1
        else if (sTemp == "S" && validDefect(hbuf, ln))
            rvSum.suggestion = rvSum.suggestion + 1

        ln = ln + 1
    }

    /* summary the satus */
    ln = 0
    while (True)
    {
        sel = SearchInBuf(hbuf, "^Status\\s+:\\s+", ln, 0, 1, 1, 0)
        if (sel == null) break

        ln = sel.lnFirst
        col = sel.ichLim
        s = GetBufLine(hbuf, ln)      
        sTemp = strmid(s, col, col+1)
        sTemp = toupper(sTemp);

        if (sTemp == "O") rvSum.open = rvSum.open + 1
        else if (sTemp == "C")  rvSum.closed = rvSum.closed + 1
        else if (sTemp == "R")  rvSum.rejected = rvSum.rejected + 1
        else if (sTemp == "D")
        {
            sTemp = strmid(s, col+1, col+2)
            sTemp = toupper(sTemp);
            
            if (sTemp == "U")  rvSum.duplicate = rvSum.duplicate + 1
               else if (sTemp == "I")  rvSum.discuss = rvSum.discuss + 1                    
        }
        
        ln = ln + 1
    }

    /* summary the categories */
    /*ln = 0
    while (True)
    {
        norej = norejected(hbuf, ln);
        notsug = notsuggestion(hbuf, ln)
        sel = SearchInBuf(hbuf, "^Category\\s+:\\s+", ln, 0, 1, 1, 0)
        if (sel == null) break

        ln = sel.lnFirst
        col = sel.ichLim
        s = GetBufLine(hbuf, ln)
        if ( col+2 > strlen(s)) 
        {
            msg("Please write categories!")
            return
        }
        sTemp = strmid(s, col, col+2)
            sTemp = toupper(sTEmp);

        if (sTemp == "SY" && norej && notsug) rvSum.SysReq = rvSum.SysReq + 1
            else if (sTemp == "SD" && norej && notsug)  rvSum.SDes = rvSum.SDes + 1
            else if (sTemp == "SR" && norej && notsug)  rvSum.SRS = rvSum.SRS + 1
            else if (sTemp == "HL" && norej && notsug)  rvSum.HLD = rvSum.HLD + 1
            else if (sTemp == "LL" && norej && notsug)  rvSum.LLD = rvSum.LLD + 1
            else if (sTemp == "TP" && norej && notsug)  rvSum.TP = rvSum.TP + 1
            else if (sTemp == "CO" && norej && notsug)  rvSum.Code = rvSum.Code + 1
            else if (sTemp == "DO" && norej && notsug)  rvSum.Docs = rvSum.Docs + 1
            else if (sTemp == "OT" && norej && notsug)  rvSum.Others = rvSum.Others + 1

            ln = ln + 1
    }*/

    if ( rvSum.major == rvSum0.major && rvSum.general == rvSum0.general && rvSum.suggestion == rvSum0.suggestion 
         &&rvSum.open == rvSum0.open && rvSum.closed == rvSum0.closed && rvSum.rejected == rvSum0.rejected 
         &&rvSum.duplicate == rvSum0.duplicate && rvSum.discuss == rvSum0.discuss 
        /*&&
        rvSum.SysReq == rvSum0.SysReq && rvSum.SDes == rvSum0.SDes &&
        rvSum.SRS == rvSum0.SRS && rvSum.HLD == rvSum0.HLD &&
        rvSum.LLD == rvSum0.LLD && rvSum.TP == rvSum0.TP &&
        rvSum.Code == rvSum0.Code && rvSum.Docs == rvSum0.Docs &&
        rvSum.Others == rvSum0.Others*/ )
        return
    else
    {
        delSummary(hbuf)
        setReviewSummary(hbuf, rvSum)
    }
}

macro getReviewSummary(hbuf)
{
    sel = SearchInBuf(hbuf, "^Summary$", 0, 0, 1, 1, 0)
    rvSum = "major=\"0\";general=\"0\";suggestion=\"0\";open=\"0\";closed=\"0\";rejected=\"0\";duplicate=\"0\";discuss=\"0\";revieweffort=\"0\";reworkeffort=\"0\";reviewsize=\"0\""

    if (sel == null)
        return rvSum

    /* get severity summary */
    ln = sel.lnFirst + 2
    sel = SearchInBuf(hbuf, "^Major\\s+:\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.major = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^General\\s+:\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.general = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^Suggestion\\s+:\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.suggestion = strmid(sLine, col, strlen(sLine))

    /* get status summary */
    sel = SearchInBuf(hbuf, "^Open\\s+:\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.open = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^Closed\\s+:\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.closed = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^Duplicated\\s+:\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.duplicate = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^Discuss\\s+:\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.discuss = strmid(sLine, col, strlen(sLine))
  

    /* get categories summary */
    /*sel = SearchInBuf(hbuf, "^SysReq\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.SysReq = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^SDes\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.SDes = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^SRS\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.SRS = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^HLD\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.HLD = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^LLD\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.LLD = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^TP\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.TP = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^Code\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.Code = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^Docs\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.Docs = strmid(sLine, col, strlen(sLine))

    sel = SearchInBuf(hbuf, "^Others\\s+=\\s+", ln, 0, 1, 1, 0)
    col = sel.ichLim
    sLine = GetBufLine(hbuf, sel.lnFirst)
    rvSum.Others = strmid(sLine, col, strlen(sLine))*/

    return rvSum
}

macro setReviewSummary(hbuf, rvSum)
{
    AppendBufLine(hbuf, "Summary")
    AppendBufLine(hbuf, "-------------------------------------------------------------------------")

    /* Defects sumary */
    AppendBufLine(hbuf, "[Defects Summary]")
    s = Cat("Total defects : ", rvSum.major + rvSum.general + rvSum.suggestion)
    AppendBufLine(hbuf, s)
    s = Cat("Major         : ", rvSum.major)
    AppendBufLine(hbuf, s)
    s = Cat("General       : ", rvSum.general)
    AppendBufLine(hbuf, s)
    s = Cat("Suggestion    : ", rvSum.suggestion)
    AppendBufLine(hbuf, s)

    /* Status sumary */
    AppendBufLine(hbuf, "")
    AppendBufLine(hbuf, "[Status Summary]")
    s = Cat("Open          : ", rvSum.open)
    AppendBufLine(hbuf, s)
    s = Cat("Closed        : ", rvSum.closed)
    AppendBufLine(hbuf, s)
    s = Cat("Rejected      : ", rvSum.rejected)
    AppendBufLine(hbuf, s)
    s = Cat("Duplicated    : ", rvSum.duplicate)
    AppendBufLine(hbuf, s)
    s = Cat("Discuss       : ", rvSum.discuss)
    AppendBufLine(hbuf, s) 


}

macro delSummary(hbuf)
{
    sel = SearchInBuf(hbuf, "^Summary$", 0, 0, 1, 1, 0)

    if (sel == null)
        return 
    else
    {
        ln = sel.lnFirst
        LineCount = GetBufLineCount(hbuf) - 1

        while(LineCount >= ln)
        {
            DelBufLine(hbuf, LineCount)
            LineCount = LineCount -1;
        }
    }
}

macro validDefect(hbuf, ln)
{
    sel = SearchInBuf(hbuf, "^Status\\s+:\\s+", ln, 0, 1, 1, 0)
    if (sel == null) return True;

    ln = sel.lnFirst
    col = sel.ichLim
    s = GetBufLine(hbuf, ln)

    sTemp = strmid(s, col, col+1)
    sTemp = toupper(sTemp);

    if (sTemp == "R") return  False;
    else if (sTemp == "D")
    {
         sTemp = strmid(s, col+1, col+2)
         sTemp = toupper(sTemp);

         if (sTemp == "U") return  False;    
    }

   
    return True;
}

macro notsuggestion(hbuf, ln)
{
    sel = SearchInBuf(hbuf, "^Severity\\s+:\\s+", ln, 0, 1, 1, 0)
    if (sel == null) return True;

    ln = sel.lnFirst
    col = sel.ichLim
    s = GetBufLine(hbuf, ln)
    sTemp = strmid(s, col, col+1)
    sTemp = toupper(sTEmp);

    if (sTemp == "S") return  False;

    return True;
}
```
