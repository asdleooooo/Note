## cookie、session、token

cookie存在的意义，就是标识请求，标识用户，存放token，状态保持，让后端服务器认识这个请求是同一个人，cookie是一个前端的技术，session是一个后端的技术，后端创建一个session，将session_key传递给前端的cookie，不会像之前那样把用户的信息明文存放在前端的cookie中，后期又出现了token加密

