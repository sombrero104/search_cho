# search_cho
초성검색



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
    
    
 ======================================================
 
 
 
 package com.inca.test;

public class Cho_Test_003 {
	
	public static char getCho(String word) {
		char[] choArray = {
				'ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ'
				, 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ'
		};
		char firstWord = word.charAt(0);
		System.out.println("# firstWord: " + firstWord); // test
		if(firstWord >= 0xAC00) { // 0xAC00 => 한글 시작점 '가'
			int uniVal = firstWord - 0xAC00;
			System.out.println("# uniVal: " + uniVal); // test
			int cho = ((uniVal - (uniVal % 28)) / 28) / 21;
			System.out.println("# cho: " + cho); // test
			return choArray[cho];
		} else {
			System.out.println("# It is not syllable.");
		}
		return 0;
	}
	
	public static void main(String[] args) {
		String name = "and";
		// ㄱ 가 각
		System.out.println("# getCho(name): " + getCho(name));
		
		/*char cho = 'ㄱ';
		char jung = 'ㅏ';
		char jong = 'ㄱ';*/
		
		/*char temp = (0xAC00 + 21*28*0 + 28*0 + 1);
		System.out.println(temp);*/
		/*char temp = (0xAC00 + 21*28*2 + 28*0 + 0);
		System.out.println(temp);*/
		
	}
	
}
