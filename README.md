

1. 영문일 경우 (It is not syllable.)

	like '%a%'
	
	like '%and%'
	

2. 한글일 경우 

	(2-1) 초성일 경우 (It is not syllable.)
	
		rlike '^ㄱ' or (NAME >= '가' and NAME < '나')
		
		rlike '^ㅌ' or (NAME >= '타' and NAME < '파')
		
		

	(2-2) 음절일 경우 
	
		like '%가%'
		
		like '%각%'
		

	=================================================================================
	
	
	// search cho
	SearchWordUtil searchWordUtil = new SearchWordUtil();
	boolean isCho = searchWordUtil.isCho(searchName);
	paramMap.put("isCho", isCho); // cho true/false
	if(isCho) {
		char choSyllable = searchWordUtil.getSyllable(searchName); // 첫번째 음절
		paramMap.put("choSyllable", choSyllable);
		if(choSyllable != '하') {
			char choNextSyllable = searchWordUtil.getNextSyllable(searchName); // 해당 음절 다음 음절
			paramMap.put("choNextSyllable", choNextSyllable);
		}
	} else {
		boolean isOneWord = (searchName.length() > 1) ? false : true;
		paramMap.put("isOneWord", isOneWord);
	}
	paramMap.put("searchName", searchName);
	
	=================================================================================
	
	package com.inca.common.util;

	import java.util.Arrays;

	public class SearchWordUtil {
		char[] choArray = { 'ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ' };
		
		/**
		 * 한글 초성 여부
		 * @param String word
		 * @return boolean
		 */
		public boolean isCho(String word) {
			boolean isCho = false;
			if(word.length() == 1) {
				char firstWord = word.charAt(0);
				if(firstWord < 0xAC00) {
					if(Arrays.binarySearch(choArray, firstWord) > -1) {
						isCho = true;
					}
				}
			}
			return isCho;
		}
		
		/**
		 * 첫번째 음절 반환
		 * @param String word
		 * @return char
		 */
		public char getSyllable(String word) {
			char cho = word.charAt(0);
			return (char)(0xAC00 + 21*28*(Arrays.binarySearch(choArray, cho)) + 28*0 + 0);
		}
		
		/**
		 * 해당 음절 다음 음절 반환
		 * @param String word
		 * @return char
		 */
		public char getNextSyllable(String word) {
			char cho = word.charAt(0);
			int nextSyllableIndex = Arrays.binarySearch(choArray, cho)+1;
			char[] exCharArray = { 'ㄱ', 'ㄷ', 'ㅂ', 'ㅅ', 'ㅈ' };
			if(Arrays.binarySearch(exCharArray, cho) > -1) {
				nextSyllableIndex++;
			}
			return (char)(0xAC00 + 21*28*nextSyllableIndex + 28*0 + 0);
		}
		
	}

	=================================================================================

	<input type="text" id="searchName" name="searchName" value="" class="form-control" style="width:230px;height:25px;padding:5px;" onkeyup="getGameInfoList()" />

	=================================================================================
	
	<if test="(searchName != null) and (searchName != '')">
				<choose>
					<when test="isCho == true">
						<choose>
							<when test="choNextSyllable != null">
								AND NAME rlike concat('^', #{searchName}) or (NAME <![CDATA[>=]]> #{choSyllable} and NAME <![CDATA[<]]> #{choNextSyllable})
							</when>
							<otherwise>
								AND NAME rlike concat('^', #{searchName}) or (NAME <![CDATA[>=]]> #{choSyllable})
							</otherwise>
						</choose>
					</when>
					<otherwise>
						<choose>
							<when test="isOneWord == true">
								AND NAME like concat(#{searchName}, '%')
							</when>
							<otherwise>
								AND NAME like concat('%', #{searchName}, '%')
							</otherwise>
						</choose>
					</otherwise>
				</choose>
			</if>
	
	=================================================================================
	
	var paramData = { searchWord : '' };
	$.ajax({
	        type : 'POST',
	        url : '/search',
	        dataType : 'json',
	        data : paramData,
	        cache : false,
	        success : function (data) {
	        },
	        error : function(request, status, error) { console.log("* code:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error); }
	    });
	}
	
	=================================================================================
	
	
	
	
	
	
	
	
	
	
	
	
	
	
