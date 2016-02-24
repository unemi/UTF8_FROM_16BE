# UTF8_FROM_16BE
    // A simple C function to convert UTF16BE to UTF8 string
    void utf8_from_16BE(unsigned char *utf8, const unsigned short *utf16, long nChars) {
    	int idx = 0;
	    for (int i = 0; i < nChars; i ++) {
		    unsigned long code = NSSwapShort(utf16[i]); // NSSwapShort is OS X Cocoa function to swap bytes
		    if (code >= 0xd800 && code < 0xdc00) {
			    unsigned long code2 = NSSwapShort(utf16[++ i]);
    			code = (((code & 0x3c0) + 1) << 10) + ((code & 0x3f) << 10) + (code2 & 0x3ff);
	    	}
		    if (code < 0x7f) utf8[idx ++] = code;
    		else {
	    		unsigned char tmp[6]; int j;
		    	for (j = 0; j < 6 && code != 0; j ++) {
			    	tmp[j] = 0x80 + (code & 0x3f);
    				code >>= 6;
	    		}
		    	for (int k = 0; k < j; k ++) utf8[idx + k] = tmp[j - 1 - k];
    		    	utf8[idx] |= (0xfc << (6 - j));
	    		idx += j;
		    }
	    }
	    utf8[idx] = 0;
    }
