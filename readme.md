1. database.py
    ```
    from sqlalchemy import create_engine
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy.orm import sessionmaker

    SQLALCHEMY_DATABASE_URL = "postgresql://postgres:raziel@localhost/test_db"
    # SQLALCHEMY_DATABASE_URL = "postgresql://user:password@postgresserver/db"

    engine = create_engine(SQLALCHEMY_DATABASE_URL)

    SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

    Base = declarative_base()
   ```
2. models.py
    ```
    from sqlalchemy import Column, Integer, String

    from database import Base

    class User(Base):
        __tablename__ = "users"
    
        id = Column(Integer, primary_key=True, index=True, nullable=False)
        email = Column(String, unique=True, index=True, nullable=False)
        password = Column(String, nullable=False)
   ```
3. main.py
    ```
   from fastapi import FastAPI
    import models
    from database import engine
    
    models.Base.metadata.create_all(bind=engine) # this line is important for creating the database table for the first time
    
    app = FastAPI()
    
    
    @app.get("/")
    async def root():
    return {"message": "Hello World"}```