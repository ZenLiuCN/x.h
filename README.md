# x.h
C macro tuples , dig and built with Gemini (free Tier 😃) , and coding by myself.
# Usage
Copy and parse below code into browser console or any js eval (not test with quickjs, but should work)
```js

const A = (n, fn) => Array(n).fill(0).map((_, i) => fn(i)),V = "__VA_ARGS__", N = "\n", C = ",", H= "X_HDR",R= "X_REST",E = "", M = 32;
((typeof copy === 'function') ? (R) => { copy(R); console.log("✅ Done with copy"); } 
: (navigator&&navigator.clipboard?.writeText) ? (R) => navigator.clipboard.writeText(R).then(() => console.log("✅ Done with navigator"))
.catch(err => console.error("❌ Fail: ", err)) : (R) =>console.log(R))([
   `#ifndef H_X_H`,
   `#define H_X_H`,
   `#include <stdarg.h> // IWYU pragma: keep`,
   `// clang-format off`,
   `#define X_TUPLE(...) (__VA_ARGS__)`,
   `#define X_UNPACK(...) __VA_ARGS__`,
   `#define X_UNPACK_COMMA(...) ,##__VA_ARGS__`,
   `#define X_NEW(N, NEW, C)({ __typeof__(*(N))* _ptr = NEW(__typeof__(*(N))); if (_ptr) { *(_ptr) = (__typeof__(*(_ptr)))X_UNPACK C; } _ptr; })`,
   `#define X_ID(X) X`,
   `#define X_STR_EXP(x) #x`,
   `#define X_STR(x) X_STR_EXP(x)`,
   `#define X_EVAL(...) __VA_ARGS__`,
   `#define X_PACK(...) (__VA_ARGS__)`,
   `#define X_HDR(a, ...) a`,
   `#define X_REST(a, ...) __VA_ARGS__`,
   `#define _X_CONCAT_INNER(a, b) a##b`,
   `#define X_CONCAT(a, b) _X_CONCAT_INNER(a, b)`,
   A(M, i =>i<1?``:`#define X_APPLY${i==1?``:`_${i}`}(${A(i,j=>`A`+j).join(C)},f) f(${A(i,j=>`A`+j).join(C)})`).join(N),
   ``,
   `#define _X_LIST_IDX ${A(M, i => i).join(C)}`,
   `#define _X_ARG_N(${A(M, i => `_` + i).join(C)}, N, ...) N`,
   `#define _X_COUNT_RAW(...) _X_ARG_N(${V}, ${A(M, i => M-i).join(C)},0)`,
   `#define X_COUNT(...) _X_COUNT_RAW(0__VAR_OPT__(,)${V})`,
   A(M + 1, i => `#define _X_MAP_STEP_${i}(M, ...) ${i>0?`X_APPLY(${H}(${V}),M)`:E} ${i > 1 ? `_X_MAP_STEP_${i-1}(M, ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_MAP_IDX_STEP_${i}(M,LI, ...) ${i>0?`X_APPLY_2( ${H} LI,${H}(${V}),M)`:E} ${i > 1 ? `_X_MAP_IDX_STEP_${i-1}(M , (${R} LI) , ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_MAP_ARG_STEP_${i}(M,ARG, ...) ${i>0?`X_APPLY_2(ARG,${H}(${V}),M)`:E} ${i > 1 ? `_X_MAP_ARG_STEP_${i-1}(M ,ARG , ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_MAP_ARG_IDX_STEP_${i}(M,ARG,LI, ...) ${i>0?`X_APPLY_3(ARG, ${H} LI,${H}(${V}),M)`:E} ${i > 1 ? `_X_MAP_ARG_IDX_STEP_${i-1}(M ,ARG, (${R} LI) , ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_STEP_${i}(M, ...) ${i>0?`X_APPLY(M, ${H}(${V}))`:E} ${i > 1 ? `_X_STEP_${i-1}(M, ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_STEP_2_${i}(M, ARG, ...) ${i>0?`X_APPLY_2(M, ARG, ${H}(${V}))`:E} ${i>1 ? `_X_STEP_2_${i-1}(M, ARG, ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_STEP_IDX_${i}(M, LI, ...) ${i>0?`X_APPLY_2(M, ${H} LI, ${H}(${V}))`:E} ${i > 1 ? `_X_STEP_IDX_${i-1}(M, (${R} LI), ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_STEP_2_IDX_${i}(M, ARG, LI, ...) ${i>0?`X_APPLY_3(M, ARG, ${H} LI, ${H}(${V}))`:E} ${i>1? `_X_STEP_2_IDX_${i-1}(M, ARG, (${R} LI), ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_JOIN_STEP_${i}(SEP, ...) ${i>0?`${H}(${V})`:E} ${i > 1 ? `SEP _X_JOIN_STEP_${i-1}(SEP, ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_TU_AT_${i}(${A(i+1, x => `a${x}`).join(C)}, ...) a${i}`).join(N),
   A(M + 1, i => `#define _X_TU_TAKE_${i}(${A(i, j => `a${j}`).join(C)}${i > 0 ? C : E} ...) (${A(i, j => `a${j}`).join(C)})`).join(N),
   A(M + 1, i => `#define _X_TU_SKIP_${i}(${A(i, j => `_` + j).join(C)}${i > 0 ? C : E} ...) (${V})`).join(N),
   A(M + 1, i => `#define _X_TU_REV_${i}(${A(i, j => `a` + j).join(C)}) (${A(i, j => `a` + (i - j - 1)).join(C)})`).join(N),
   A(M + 1, i => `#define _X_TU_MAP_STEP_${i}(M, ...) ${i>0?`X_APPLY(${H}(${V}),M)`:E}${i > 1 ? `, _X_TU_MAP_STEP_${i-1}(M, ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_TU_MAP_ARG_STEP_${i}(M,ARG, ...) ${i>0?`X_APPLY_2(ARG,${H}(${V}),M)`:E}${i > 1 ? `, _X_TU_MAP_ARG_STEP_${i-1}(M,ARG, ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_TU_MAP_IDX_STEP_${i}(M,LI, ...) ${i>0?`X_APPLY_2(${H} LI,${H}(${V}), M)`:E}${i > 1 ? ` , _X_TU_MAP_IDX_STEP_${i-1}(M , (${R} LI) , ${R}(${V}))` : E}`).join(N),
   A(M + 1, i => `#define _X_TU_MAP_ARG_IDX_STEP_${i}(M,ARG,LI, ...) ${i>0?`X_APPLY_3(ARG, ${H} LI,${H}(${V}),M)`:E}${i > 1 ? ` , _X_TU_MAP_ARG_IDX_STEP_${i-1}(M ,ARG, (${R} LI) , ${R}(${V}))` : E}`).join(N),
   A(M + 1, i =>`#define _X_TU_PICK_IDX_${i}(FIELDS, ...) ${i>0?`X_TUPLE_AT(FIELDS,${H}(${V}))`:E} ${i > 1 ? `, _X_TU_PICK_IDX_${i-1}(FIELDS, ${R}(${V}))` : E}`).join(N),
   `// clang-format on
// expand max to ${M} varidc args of X micro to apply
#define X_EACH(M, ...) X_CONCAT(_X_STEP_, X_COUNT(__VA_ARGS__))(M, __VA_ARGS__)
// expand max to ${M} va_args of X micro to apply with one extra arg
#define X_EACH_ARG(M, ARG, ...) X_CONCAT(_X_STEP_2_, X_COUNT(__VA_ARGS__))(M, ARG, __VA_ARGS__)
// expand max to ${M} va_args of X micro to apply with index
#define X_EACH_IDX(M, ...) X_CONCAT(_X_STEP_IDX_, X_COUNT(__VA_ARGS__))(M, (_X_LIST_IDX), __VA_ARGS__)
// expand max to ${M} va_args of X micro to apply with one arg and index
#define X_EACH_ARG_IDX(M, ARG, ...) X_CONCAT(_X_STEP_2_IDX_, X_COUNT(__VA_ARGS__))(M, ARG, (_X_LIST_IDX), __VA_ARGS__)
// map each value, max to ${M}
#define X_MAP(FN, ...) X_CONCAT(_X_MAP_STEP_, X_COUNT(__VA_ARGS__))(FN, __VA_ARGS__)
// map each value , max to ${M}
#define X_MAP_ARG(FN, ARG, ...) X_CONCAT(_X_MAP_ARG_STEP_, X_COUNT(__VA_ARGS__))(FN, ARG, __VA_ARGS__)
// map each value , max to ${M}
#define X_MAP_IDX(FN, ...) X_CONCAT(_X_MAP_IDX_STEP_, X_COUNT(__VA_ARGS__))(FN, (_X_LIST_IDX), __VA_ARGS__)
// map each value, max to ${M}
#define X_MAP_ARG_IDX(FN, ARG, ...) X_CONCAT(_X_MAP_ARG_IDX_STEP_, X_COUNT(__VA_ARGS__))(FN, ARG, (_X_LIST_IDX), __VA_ARGS__)
// join each value, max to ${M}
#define X_JOIN(SEP, ...) X_CONCAT(_X_JOIN_STEP_, X_COUNT(__VA_ARGS__))(SEP, __VA_ARGS__)
// map each value of TUPLE
#define _DO_X_TUPLE_MAP(FN, ...) (X_CONCAT(_X_TU_MAP_STEP_, X_COUNT(__VA_ARGS__))(FN, __VA_ARGS__))
#define X_TUPLE_MAP(TUPLE, FN) _DO_X_TUPLE_MAP(FN, X_UNPACK TUPLE)
// map each value of TUPLE max to ${M} element
#define _DO_X_TUPLE_MAP_IDX(FN, ...) (X_CONCAT(_X_TU_MAP_IDX_STEP_, X_COUNT(__VA_ARGS__))(FN, (_X_LIST_IDX), __VA_ARGS__))
#define X_TUPLE_MAP_IDX(TUPLE, FN) _DO_X_TUPLE_MAP_IDX(FN, X_UNPACK TUPLE)
// map each value of TUPLE with one extra arg TUPLE max to ${M} element
#define _DO_X_TUPLE_MAP_ARG(FN, ARG, ...) (X_CONCAT(_X_TU_MAP_ARG_STEP_, X_COUNT(__VA_ARGS__))(FN, ARG, __VA_ARGS__))
#define X_TUPLE_MAP_ARG(TUPLE, ARG, FN) _DO_X_TUPLE_MAP_ARG(FN, ARG, X_UNPACK TUPLE)
// map each value of TUPLE with one extra arg and index
#define _DO_X_TUPLE_MAP_ARG_IDX(FN, ARG, ...) (X_CONCAT(_X_TU_MAP_ARG_IDX_STEP_, X_COUNT(__VA_ARGS__))(FN, ARG, (_X_LIST_IDX), __VA_ARGS__))
#define X_TUPLE_MAP_ARG_IDX(TUPLE, ARG, FN) _DO_X_TUPLE_MAP_ARG_IDX(FN, ARG, X_UNPACK TUPLE)
// pick value at index of TUPLE max to ${M} element
#define X_TUPLE_AT(TUPLE, IDX) X_CONCAT(_X_TU_AT_, IDX) TUPLE
// take value of first N in TUPLE max to ${M} element
#define X_TUPLE_TAKE(TUPLE, N) X_CONCAT(_X_TU_TAKE_, N) TUPLE
// skip value of first N in TUPLE max to ${M} element
#define X_TUPLE_SKIP(TUPLE, N) X_CONCAT(_X_TU_SKIP_, N) TUPLE
// slice value from (M,N) in TUPLE  max to ${M} element
#define X_TUPLE_SLICE(TUPLE, M, N) X_TUPLE_TAKE(X_TUPLE_SKIP(TUPLE, M), N)
// append value to TUPLE (no element limits except the cc's limit)
#define X_TUPLE_APPEND(TUPLE, ...) (X_UNPACK TUPLE, ##__VA_ARGS__)
// prepend value to TUPLE (no element limits except the cc's limit)
#define X_TUPLE_PREPEND(TUPLE, ...) (__VA_ARGS__, X_UNPACK TUPLE)
// revrese element in TUPLE
#define X_TUPLE_REVERSE(TUPLE) X_CONCAT(_X_TU_REV_, X_COUNT TUPLE) TUPLE
// join element in TUPLE with ", "
#define X_TUPLE_JOIN(TUPLE) X_JOIN(", ", X_UNPACK TUPLE)
// join element in TUPLE max to ${M} element
#define X_TUPLE_JOINS(TUPLE, SEP) X_JOIN(SEP, X_UNPACK TUPLE)
//pick element in TUPLE with indexes max to ${M} element
#define X_TUPLE_PICK(FIELDS, ...) (X_CONCAT(_X_TU_PICK_IDX_, X_COUNT(__VA_ARGS__))(FIELDS, __VA_ARGS__))
#define X_COMMA ,
#define X_SEMI ;
#endif //H_X_H`,
 ].join(N));
 ```
the copy the output, or just to parse in header file.
# Idea comes
inspired when coding a little project.
# Example
```C
#define SQL_READ_RAW(x) x              
#define SQL_READ_JSON(x) "json(" x ")" 
//==== (SQL_TYPE,DB_TYPE,C_TYPE,PLACE_HOLDER(?),READER(%s)) =============
#define SQL_U_I64 (int64, "INTEGER", SQL_C_I64, "?", SQL_READ_RAW)
#define SQL_U_U64 (int64, "INTEGER", SQL_C_U64, "?", SQL_READ_RAW)
#define SQL_U_I32 (int, "INTEGER", SQL_C_I32, "?", SQL_READ_RAW)
#define SQL_U_U32 (int, "INTEGER", SQL_C_U32, "?", SQL_READ_RAW)
#define SQL_U_BLOB (blob, "BLOB", SQL_C_BLOB, "?", SQL_READ_RAW)
#define SQL_U_REAL (double, "REAL", SQL_C_REAL, "?", SQL_READ_RAW)
#define SQL_U_TIMESTAMP (int64, "INTEGER", SQL_C_TIMESTAMP, "?", SQL_READ_RAW)
#define SQL_U_TIMESTAMP_DEFAULT (int64, "INTEGER DEFAULT(unixepoch())", SQL_TIMESTAMP, "?", SQL_READ_RAW)
// note: use bool
#define SQL_U_BOOL (int, "INTEGER", bool, "?", SQL_READ_RAW)
#define SQL_U_STR (text, "TEXT", SQL_C_TEXT, "?", SQL_READ_RAW)
#define SQL_U_TEXT (text, "TEXT", SQL_C_BIG_TEXT, "?", SQL_READ_RAW)
#define SQL_U_JSON (text, "TEXT", SQL_C_JSON, "?", SQL_READ_RAW)
#define SQL_U_JSONB (blob, "BLOB", SQL_C_JSONB, "jsonb(?)", SQL_READ_JSON)
//==== (FIELD_NAME,COLUMN_NAME,SQL_TYPE,DB_TYPE,C_TYPE,PLACE_HOLDER(?),READER(COLUMN_NAME maybe with function))========
#define SQL_F_ID32 (id, "id", int, "INTEGER PRIMARY KEY CHECK(id <= 2147483647)", SQL_C_I32, "?", SQL_READ_RAW, true)
#define SQL_F_ID64 (id, "id", int64, "INTEGER PRIMARY KEY", SQL_C_I64, "?", SQL_READ_RAW, true)

#define SQL_FIELD_TUPLE_X(X, T)                                                                                        \
    X(T, SQL_TYPE, 0)                                                                                                  \
    X(T, DB_TYPE, 1)                                                                                                   \
    X(T, C_TYPE, 2)                                                                                                    \
    X(T, HOLDER, 3)                                                                                                    \
    X(T, READER, 4)

X_ENUM_SN(SQL_FU, -1, SQL_FIELD_TUPLE_X);

// BIND= NAME(out,row,value)
#define SQL_FIELD_DEFINE_TUPLE_X(X, T)                                                                                 \
    X(T, FIELD, 0)                                                                                                     \
    X(T, DB_NAME, 1)                                                                                                   \
    X(T, SQL_TYPE, 2)                                                                                                  \
    X(T, DB_TYPE, 3)                                                                                                   \
    X(T, C_TYPE, 4)                                                                                                    \
    X(T, HOLDER, 5)                                                                                                    \
    X(T, READER, 6)                                                                                                    \
    X(T, MK_ID, 7)                                                                                                     \
    X(T, BIND, 8)

X_ENUM_SN(SQL_FD, -1, SQL_FIELD_DEFINE_TUPLE_X);


#define SQL_MODEL_DEFINE_TUPLE_X(X, T)                                                                                 \
    X(T, STRUCT, 0)                                                                                                    \
    X(T, TABLE_NAME, 1)                                                                                                \
    X(T, FIEDLS, 2)

X_ENUM_SN(SQL_MODEL, -1, SQL_MODEL_DEFINE_TUPLE_X);

#define _SQL_FIELD_(TYPE_NAME, FD) X_TUPLE_AT(FD, 4) X_TUPLE_AT(FD, 0);

#define _SQL_COLUMN_DEF_(TYPE_NAME, N, FD) X_TUPLE_AT(FD, 1) " " X_TUPLE_AT(FD, 3)

#define _SQL_FIELD_INDEX_(TYPE_NAME, N, FD) X_CONCAT(TYPE_NAME##_column_, X_EVAL(X_TUPLE_AT(FD, 0))) = N,

/* --------------------------------------------------------------------------
 * @brief build struct define
 * @param TYPE_NAME: the type name
 * @param DB_TABLE_NAME: the table name
 * @param ASSOC_CREATE: extra creation SQL, eg FTS vitrual table and triggers
 * @param va_args: field tuples
 * (FIELD_NAME,DB_NAME_STR,SQL_TYPE,DB_MODIFIER,C_TYPE,PLACE_HOLDER,READER,BIND_CODE,ID,extra...)
 * -------------------------------------------------------------------------- */
#define SQL_BIND(ASSOC_CREATE, MODEL)                                                                                  \
    typedef struct {                                                                                                   \
        X_TUPLE_JOINS(X_TUPLE_MAP_ARG(X_TUPLE_AT(MODEL, 2), X_TUPLE_AT(MODEL, 0), _SQL_FIELD_), )                      \
    } X_TUPLE_AT(MODEL, 0);                                                                                            \
    typedef enum {                                                                                                     \
        X_MAP_ARG_IDX(_SQL_FIELD_INDEX_, X_TUPLE_AT(MODEL, 0), X_UNPACK X_TUPLE_AT(MODEL, 2))                          \
    } X_CONCAT(X_TUPLE_AT(MODEL, 0), _columns);                                                                        \
    const char* X_CONCAT(X_TUPLE_AT(MODEL, 0), _create_sql) =                                                          \
        "CREATE TABLE IF NOT EXISTS " X_TUPLE_AT(MODEL, 1) "(" X_TUPLE_JOIN(X_TUPLE_MAP_ARG_IDX(                       \
            X_TUPLE_AT(MODEL, 2), X_TUPLE_AT(MODEL, 0), _SQL_COLUMN_DEF_)) ");" X_UNPACK ASSOC_CREATE;


/* --------------------------------------------------------------------------
 * @brief build query
 * @param DB: the sql
 * @param out: the structure pointer to recive
 * @param DB_TABLE_NAME: the table name
 * @param FIELDS: field tuples
 * (FIELD_NAME,DB_NAME_STR,SQL_TYPE,DB_MODIFIER,C_TYPE,PLACE_HOLDER,READER,BIND,extra...)[]
 * BIND should be as BINDER(out_ptr, field, row, col, values)
 * BIND_CODE is an macro of (out_ptr)=>bind_code
 * @param PARAM_LIST: tuple of param fields index
 * @param PICK_LIST: tuple of result fields index
 *
 * -------------------------------------------------------------------------- */


#define _APPLY_FUNC(FUNC, COL_NAME) FUNC(COL_NAME)

#define _SQL_WHERE_ITEM(FIELD)

#define _SQL_COL_EQ_PARAM(FIELD) X_TUPLE_AT(FIELD, 1) " = " X_TUPLE_AT(FIELD, 5) // field= place_holder

#define _SQL_SELECT_COL(FIELD) X_TUPLE_AT(FIELD, 6)(X_TUPLE_AT(FIELD, 1))

#define _MK_QUERY(TABLE, FIELDS, PARAM_LIST, PICK_LIST)                                                                \
    "SELECT " X_TUPLE_JOIN(                                                                                            \
        X_TUPLE_MAP(X_TUPLE_PICK(FIELDS, X_UNPACK PICK_LIST), _SQL_SELECT_COL))                                         \
         " FROM " TABLE " WHERE "                                                                                     \
         X_TUPLE_JOINS(X_TUPLE_MAP(X_TUPLE_PICK(FIELDS, X_UNPACK PARAM_LIST),_SQL_COL_EQ_PARAM)," AND ")

#define _SQL_PARAM_DECL(FIELD) X_TUPLE_AT(FIELD, 4) X_TUPLE_AT(FIELD, 0)

#define _MK_PARAMS(FIELDS, PARAM_LIST)                                                                                 \
    X_TUPLE_JOINS(X_TUPLE_MAP(X_TUPLE_PICK(FIELDS, X_UNPACK PARAM_LIST), _SQL_PARAM_DECL), )

#define _SQL_TYPE_VAL(FIELD) X_CONCAT(sql_type_, X_TUPLE_AT(FIELD, 2)),

#define _MK_RESULT_TYPES(FIELDS, PICK_LIST) {X_MAP(_SQL_TYPE_VAL, X_UNPACK X_TUPLE_PICK(FIELDS, X_UNPACK PICK_LIST))}

extern void* ERROR_SQL_UNSUPPORTED_TYPE_PLEASE_USE_MANUAL_BIND(void);

#define _SQL_PARAMS(x)                                                                                                 \
    _Generic((x),                                                                                                      \
        _Bool: (sql_value){.type = sql_type_int, .i = (int64_t)(x)},                                                   \
        int8_t: (sql_value){.type = sql_type_int, .i = (int64_t)(x)},                                                  \
        uint8_t: (sql_value){.type = sql_type_int, .i = (int64_t)(x)},                                                 \
        int16_t: (sql_value){.type = sql_type_int, .i = (int64_t)(x)},                                                 \
        uint16_t: (sql_value){.type = sql_type_int, .i = (int64_t)(x)},                                                \
        int32_t: (sql_value){.type = sql_type_int, .i = (int64_t)(x)},                                                 \
        uint32_t: (sql_value){.type = sql_type_int64, .i64 = (int64_t)(x)},                                            \
        int64_t: (sql_value){.type = sql_type_int64, .i64 = (int64_t)(x)},                                             \
        uint64_t: (sql_value){.type = sql_type_int64, .i64 = (int64_t)(x)},                                            \
        float: (sql_value){.type = sql_type_double, .f64 = (double)(uintptr_t)(x)},                                    \
        double: (sql_value){.type = sql_type_double, .f64 = (double)(uintptr_t)(x)},                                   \
        char*: (sql_value){.type = sql_type_text, .nil = x == NULL, .text = (char*)(x), .len32 = strlen(x)},           \
        const char*: (sql_value){.type = sql_type_text, .nil = x == NULL, .text = (char*)(x), .len32 = strlen(x)},     \
        unsigned char*: (sql_value){.type = sql_type_text, .nil = x == NULL, .text = (char*)(x), .len32 = strlen(x)},  \
        const unsigned char*: (                                                                                        \
            sql_value){.type = sql_type_text, .nil = x == NULL, .text = (char*)(x), .len32 = strlen(x)},               \
        default: ((sql_value){.type = (uint32_t)(uintptr_t)ERROR_SQL_UNSUPPORTED_TYPE_PLEASE_USE_MANUAL_BIND()}))
#define _SQL_ARG_VAL(FIELD) _SQL_PARAMS(X_TUPLE_AT(FIELD, 0))
#define _MK_ARGS(FIELDS, PARAM_LIST)                                                                                   \
    (sql_value[]) { X_UNPACK X_TUPLE_MAP(X_TUPLE_PICK(FIELDS, X_UNPACK PARAM_LIST), _SQL_ARG_VAL) }
#define _SQL_BIND_VAL(OUT, N, FIELD) X_TUPLE_AT(FIELD, 8)(OUT, X_TUPLE_AT(FIELD, 0), row, N, values)
#define _MK_BIND_ACTION(OUT_PTR, FIELDS, PICK_LIST)                                                                    \
    X_TUPLE_JOINS(X_TUPLE_MAP_ARG_IDX(X_TUPLE_PICK(FIELDS, X_UNPACK PICK_LIST), OUT_PTR, _SQL_BIND_VAL), X_SEMI)
/* --------------------------------------------------------------------------
 * @brief auto select builder
 * @param QUERY_NAME the function name
 * @param OUT_TYPE the result holder type
 * @param MODEL TUPLE of SQL MODEL
 * @param PARAM_LIST TUPLE of parameter indexes (only for simple equal)
 * @param PICK_LIST TUPLE of result indexes
 * @param EXTRA_QUERY text for query suffix parts
 * @Note: only support for simple types as parameter, text must end with '\0'
 * -------------------------------------------------------------------------- */

#define SQL_SELECT(QUERY_NAME, OUT_TYPE, MODEL, PARAM_LIST, PICK_LIST, EXTRA_QUERY)                                    \
    static const char* QUERY_NAME##_query_sql =                                                                        \
        _MK_QUERY(X_TUPLE_AT(MODEL, 1), X_TUPLE_AT(MODEL, 2), PARAM_LIST, PICK_LIST) EXTRA_QUERY;                      \
    static inline int QUERY_NAME(sql db, OUT_TYPE* out, _MK_PARAMS(X_TUPLE_AT(MODEL, 2), PARAM_LIST)) {                \
        static const sql_type QUERY_NAME##_query_result_type[] = _MK_RESULT_TYPES(X_TUPLE_AT(MODEL, 2), PICK_LIST);    \
        int                   rc                               = 0;                                                    \
        SQL_CURSOR((return -1;), db,                                                                                   \
            sql_query(db, QUERY_NAME##_query_sql, X_COUNT PARAM_LIST, _MK_ARGS(X_TUPLE_AT(MODEL, 2), PARAM_LIST),      \
                X_COUNT PICK_LIST, QUERY_NAME##_query_result_type),                                                    \
            (_MK_BIND_ACTION(out, X_TUPLE_AT(MODEL, 2), PICK_LIST);));                                                 \
        return rc;                                                                                                     \
    }
```
With example code
```C
#define READ_I64(out, field, row, col, values)                                                                         \
    out->field = values[col].i64;                                                                                      
#define READ_I32(out, field, row, col, values)                                                                         \
    out->field = values[col].i;                                                                                        
#define READ_STR(out, field, row, col, values)                                                                         \                                                        out->field = strdup(values[col].text);                                                                             
#define READ_JSONB(out, field, row, col, values)                                                                       \
  out->field = strdup(values[col].text);                                                                             

#define DEVICE_FIELDS                                                                                                  \
    ((X_UNPACK SQL_F_ID64, READ_I64), (sn, "sn", X_UNPACK SQL_U_STR, false, READ_STR),                                 \
        (power, "power", X_UNPACK SQL_U_I32, false, READ_I32),                                                         \
        (extra, "extra", X_UNPACK SQL_U_JSONB, false, READ_JSONB))
SQL_BIND((), (device_t, "t_device", DEVICE_FIELDS))
SQL_SELECT(device_get_by_sn, device_t,(device_t, "t_device", DEVICE_FIELDS), (1), (0, 2, 3), " LIMIT 1")
```
Will get
```C

typedef struct {
    SQL_C_I64   id;
    SQL_C_TEXT  sn;
    SQL_C_I32   power;
    SQL_C_JSONB extra;
} device_t;
typedef enum {
    device_t_column_id    = 0,
    device_t_column_sn    = 1,
    device_t_column_power = 2,
    device_t_column_extra = 3,
} device_t_columns;
const char* device_t_create_sql = "CREATE TABLE IF NOT EXISTS "
                                  "t_device"
                                  "("
                                  "id"
                                  " "
                                  "INTEGER PRIMARY KEY"
                                  ", "
                                  "sn"
                                  " "
                                  "TEXT"
                                  ", "
                                  "power"
                                  " "
                                  "INTEGER"
                                  ", "
                                  "extra"
                                  " "
                                  "BLOB"
                                  ");";

static const char* device_get_by_sn_query_sql = "SELECT "
                                                "id"
                                                ", "
                                                "power"
                                                ", "
                                                "json("
                                                "extra"
                                                ")"
                                                " FROM "
                                                "t_device"
                                                " WHERE "
                                                "sn"
                                                " = "
                                                "?"
                                                " LIMIT 1";
static inline int  device_get_by_sn(sql db, device_t* out, SQL_C_TEXT sn) {
    static const sql_type device_get_by_sn_query_result_type[] = {
        sql_type_int64,
        sql_type_int,
        sql_type_blob,
    };
    int rc = 0;
    do {
        sql_cursor cursor = sql_query(db, device_get_by_sn_query_sql, 1,
             (sql_value[]){_Generic((sn),
                    _Bool: (sql_value){.type = sql_type_int, .i = (int64_t)(sn)},
                    int8_t: (sql_value){.type = sql_type_int, .i = (int64_t)(sn)},
                    uint8_t: (sql_value){.type = sql_type_int, .i = (int64_t)(sn)},
                    int16_t: (sql_value){.type = sql_type_int, .i = (int64_t)(sn)},
                    uint16_t: (sql_value){.type = sql_type_int, .i = (int64_t)(sn)},
                    int32_t: (sql_value){.type = sql_type_int, .i = (int64_t)(sn)},
                    uint32_t: (sql_value){.type = sql_type_int64, .i64 = (int64_t)(sn)},
                    int64_t: (sql_value){.type = sql_type_int64, .i64 = (int64_t)(sn)},
                    uint64_t: (sql_value){.type = sql_type_int64, .i64 = (int64_t)(sn)},
                    float: (sql_value){.type = sql_type_double, .f64 = (double)(uintptr_t)(sn)},
                    double: (sql_value){.type = sql_type_double, .f64 = (double)(uintptr_t)(sn)},
                    char*: (sql_value){.type = sql_type_text,
                         .nil                 = sn == ((void*)0),
                         .text                = (char*)(sn),
                         .len32               = strlen(sn)},
                    const char*: (sql_value){.type = sql_type_text,
                         .nil                       = sn == ((void*)0),
                         .text                      = (char*)(sn),
                         .len32                     = strlen(sn)},
                    unsigned char*: (sql_value){.type = sql_type_text,
                         .nil                          = sn == ((void*)0),
                         .text                         = (char*)(sn),
                         .len32                        = strlen(sn)},
                    const unsigned char*: (sql_value){.type = sql_type_text,
                         .nil                                = sn == ((void*)0),
                         .text                               = (char*)(sn),
                         .len32                              = strlen(sn)},
                    default: ((sql_value){
                         .type = (uint32_t)(uintptr_t)ERROR_SQL_UNSUPPORTED_TYPE_PLEASE_USE_MANUAL_BIND()}))},
             3, device_get_by_sn_query_result_type);
        if (!cursor) {
            log_log(LOG_ERROR, "src/test/msql.c", 35, "fetch cursor fail %s", sql_error(db));
            return -1;
        };
        const sql_value* values = sql_cursor_next(cursor);
        int              row    = 0;
        while (values != ((void*)0)) {
            out->id = values[0].i64;
            out->power = values[1].i;
            out->extra = strdup(values[2].text);
            values = sql_cursor_next(cursor);
            row++;
        }
        sql_cursor_close(cursor);
    } while (0);
    return rc;
}

```
# License
MIT 
