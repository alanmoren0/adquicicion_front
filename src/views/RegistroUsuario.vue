<template>
    <div class="container">

        <form action="login.html">
            <div class="form-group">
                <div class="form-group">
                    <label for="email">Correo electrónico</label>
                    <input type="email" class="form-control" id="email">
                </div>
                <div class="form-group">
                    <label for="pass">Contraseña</label>
                    <input type="password" class="form-control" id="pass">
                </div>
                <div class="form-group">
                    <label for="passc">Confirma contraseña</label>
                    <input type="password" class="form-control" id="passc">
                </div>
                <div class="form-group">
                    <label for="nombre">Nombre del usuario</label>
                    <input type="text" class="form-control" id="nombre">
                </div>
                <button type="submit" class="btn btn-success">Enviar</button>
                <button type="cancel" class="btn btn-danger">Cancelar</button>
            </div>
        </form>

    </div>
</template>