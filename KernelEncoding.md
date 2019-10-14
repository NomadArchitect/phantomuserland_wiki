# Working with text strings and characters

In short: if not defined otherwise, strings are UTF-8. Characters must be UTF-32. 
Keyboard returns UTF-32. Type is wchar_t, which is 32 bits.

## Headers

### utf8proc.h 

* ```ssize_t utf8proc_iterate(const uint8_t *str, ssize_t strlen, int32_t *dst)```

Convert from UTF-8 to UTF-32

Reads a single char from the UTF-8 sequence being pointed to by 'str'.
The maximum number of bytes read is 'strlen', unless 'strlen' is negative.                                                               

If a valid unicode char could be read, it is stored in the variable
being pointed to by 'dst', otherwise that variable will be set to -1.
In case of success the number of bytes read is returned, otherwise a
negative error code is returned.                                        

* ```ssize_t utf8proc_encode_char(int32_t uc, uint8_t *dst)```

Encodes the unicode char with the code point 'uc' as an UTF-8 string in
the byte array being pointed to by 'dst'. This array has to be at least 4 bytes long.

In case of success the number of bytes written is returned, otherwise 0.

This function does not check if 'uc' is a valid unicode code point.


### whchar.h

* ```size_t	wcslen (const wchar_t *)```

* ```size_t	wcslcat (wchar_t *, const wchar_t *, size_t)```
* ```size_t	wcslcpy (wchar_t *, const wchar_t *, size_t)```

* ```size_t	wcsnlen (const wchar_t *, size_t)```
* ```int	wcsncmp (const wchar_t *, const wchar_t *, size_t)```

* ```wchar_t	*wmemset (wchar_t *, wchar_t, size_t)```
* ```wchar_t	*wmemchr (const wchar_t *, wchar_t, size_t)```

### video/font.h

```
void w_ttfont_draw_string(
                          window_handle_t win,
                          font_handle_t font,
                          const char *s, const rgba_t color,
                          int x, int y );
```

Render UTF-8 string to window.

```
void w_ttfont_draw_string_ext(
                          window_handle_t win,
                          font_handle_t font,
                          const char *str, size_t strLen,
                          const rgba_t color,
                          int win_x, int win_y,
                          int *find_x, int find_for_char );
```

Render UTF-8 string to window. Give out X position of given character (cursor position).

```
void w_ttfont_draw_char(
                          window_handle_t win,
                          font_handle_t font,
                          const char *str, const rgba_t color,
                          int win_x, int win_y );
```

Render UTF-8 character. Not to be widely used, no kerning.

```
void w_ttfont_string_size( font_handle_t font,
                          const char *str, size_t strLen,
                          rect_t *r );
```

Calculate bounding rectangle for string.

#### Font access

```
font_handle_t w_get_system_font_ext( int font_size );
font_handle_t w_get_system_font( void );

font_handle_t w_get_system_mono_font_ext( int font_size );
font_handle_t w_get_system_mono_font( void );


font_handle_t w_get_tt_font_file( const char *file_name, int size );
font_handle_t w_get_tt_font_mem( void *mem_font, size_t mem_font_size, const char* diag_font_name, int font_size );


errno_t w_release_tt_font( font_handle_t font );
```

